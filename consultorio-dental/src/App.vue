<script>
  import Login from './components/Login.vue'
  import Signup from './components/Signup.vue'
  import Home from './components/Home.vue'
  import Navbar from './components/Navbar.vue'
  import MyReservations from './components/MyReservations.vue'
  import MakeReservation from './components/MakeReservation.vue'
  import CancelReservation from './components/CancelReservation.vue'
  import Reservations from './components/Reservations.vue'
  import Users from './components/Users.vue'
  import AdminCreateReservation from './components/AdminCreateReservation.vue'

  export default{
    components: {
      Login,
      Signup,
      Home,
      Navbar,
      MyReservations,
      MakeReservation,
      CancelReservation,
      Reservations,
      Users,
      AdminCreateReservation
    },
    beforeMount(){
      this.searchCookies()
    },
    data(){
      return{
        route: 'home',
        apiURL: 'https://consultorio--dental.herokuapp.com/',
        user: null,
        loggedIn: false,
        reservations: [],
        cancelled: [],
        finished: [],
        pastReservations: [],
        activeReservation: {},
        admin: false,
        users: []
      }
    },
    methods: {
      async fetchData(route,bodyObject){
          const resp = await fetch(this.apiURL + route, {
              method: 'post',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify(bodyObject)
          }).then(res => res.json())
          return resp
      },
      async fetchGet(route){
          const resp = await fetch(this.apiURL + route, {
              method: 'get',
              headers: {'Content-Type': 'application/json'}
          }).then(res => res.json())
          return resp
      },
      getCookie(name){
        let cookieArray = document.cookie.split(';')
        name = name + '='
        let cookieValue = ''

        for(let i = 0; i < cookieArray.length; i++){
          let cookie = cookieArray[i]

          while(cookie.charAt(0) === ' '){
            cookie = cookie.substring(1)
          }
          if(cookie.indexOf(name) === 0){
            cookieValue = cookie.substring(name.length)
          }
        }
        return cookieValue
      },
      searchCookies(){
        let cookie = document.cookie
        let sid,status
        
        if(cookie !== ''){
          sid = this.getCookie('SID')

          this.fetchData('restaurar-sesion',{SID: sid}).then(user => {
            if(user === 'Algo salió mal'){
              return
            }
            this.loadUser(user)
          })
        }
      },
      changeRoute(newRoute){
          this.route = newRoute
      },
      changeActiveReservation(newactive){
        this.activeReservation = newactive
      },
      loadUser(user){
        this.loggedIn = true
        this.user = user[0]
        this.users = []

        if(this.user.tipo_de_usuario === 1){
          this.admin = true
        }

        if(this.admin){
          this.fetchGet('obtener-reservaciones-totales').then(reservaciones => {
            this.fetchGet('obtener-reservaciones-canceladas-totales').then(canceladas => {
              this.fetchGet('obtener-reservaciones-finalizadas-totales').then(finalizadas => {
                this.fetchGet('obtener-reservaciones-pendientes-totales').then(pendientes => {
                  this.fetchGet('obtener-usuarios').then(usuarios => {
                    let pastReservations = canceladas.concat(finalizadas)
                    pastReservations.map(res => {
                      let nuevo_inicio = new Date(res.hora_inicio)
                      let nuevo_fin = new Date(res.hora_fin)
                      nuevo_inicio = new Date(nuevo_inicio - (1000*60*60*5))
                      nuevo_fin = new Date(nuevo_fin - (1000*60*60*5))
                      res.hora_inicio = nuevo_inicio.toISOString()
                      res.hora_fin = nuevo_fin.toISOString()
                      return res
                    })

                    pendientes.map(res => {
                      let nuevo_inicio = new Date(res.hora_inicio)
                      let nuevo_fin = new Date(res.hora_fin)
                      nuevo_inicio = new Date(nuevo_inicio - (1000*60*60*5))
                      nuevo_fin = new Date(nuevo_fin - (1000*60*60*5))
                      res.hora_inicio = nuevo_inicio.toISOString()
                      res.hora_fin = nuevo_fin.toISOString()
                      return res
                    })

                    this.pastReservations = pastReservations
                    this.reservations = pendientes
                    this.cancelled = canceladas
                    this.users = usuarios

                    this.changeRoute('reservations')
                  })
                })
              })
            })
          })
        }
        else{
          this.fetchData('obtener-reservaciones-pendientes',{idUsuario: this.user.id}).then(pendientes=> {
            this.fetchData('obtener-reservaciones-canceladas',{idUsuario: this.user.id}).then(canceladas => {
              this.fetchData('obtener-reservaciones-finalizadas',{idUsuario: this.user.id}).then(finalizadas => {
                let pastReservations = canceladas.concat(finalizadas)
                pastReservations.map(res => {
                  let nuevo_inicio = new Date(res.hora_inicio)
                  let nuevo_fin = new Date(res.hora_fin)
                  nuevo_inicio = new Date(nuevo_inicio - (1000*60*60*5))
                  nuevo_fin = new Date(nuevo_fin - (1000*60*60*5))
                  res.hora_inicio = nuevo_inicio.toISOString()
                  res.hora_fin = nuevo_fin.toISOString()
                  return res
                })

                pendientes.map(res => {
                  let nuevo_inicio = new Date(res.hora_inicio)
                  let nuevo_fin = new Date(res.hora_fin)
                  nuevo_inicio = new Date(nuevo_inicio - (1000*60*60*5))
                  nuevo_fin = new Date(nuevo_fin - (1000*60*60*5))
                  res.hora_inicio = nuevo_inicio.toISOString()
                  res.hora_fin = nuevo_fin.toISOString()
                  return res
                })

                this.reservations = pendientes
                this.cancelled = canceladas
                this.finished = finalizadas
                this.pastReservations = pastReservations 
                this.changeRoute('myreservations')

              })
            })
          })
        }
      },
      logout(){
        this.loggedIn = false
        this.user = null
        this.reservations = []
        this.cancelled = []
        this.finished = []
        this.pastReservations = []
        this.activeReservation = {}
        this.admin = false
        document.cookie = 'SID=;'

        location.reload()
      }
    }
  }

</script>

<template>
  <div class="App">
    <Home v-if="this.route === 'home'" :changeRoute="changeRoute" />
    <Login v-if="this.route === 'login'" :loadUser="loadUser" :changeRoute="changeRoute" :fetchGet="fetchGet" :fetchData="fetchData" />
    <Signup v-if="this.route === 'signup'" :changeRoute="changeRoute" :fetchGet="fetchGet" :fetchData="fetchData" />
    <div v-if="this.loggedIn" class="loggedInInterface">
      <Navbar :admin="admin" :route="route" :changeRoute="this.changeRoute" :logout="this.logout" />
      <MyReservations v-if="this.route === 'myreservations' && !this.admin" :changeRoute="changeRoute" :changeActiveReservation="changeActiveReservation" :pastReservations="pastReservations" :reservations="reservations" :finished="finished" :cancelled="cancelled" />
      <MakeReservation v-if="this.route === 'makereservation' && !this.admin" :changeRoute="changeRoute" :user="this.user" :fetchData="fetchData"/>
      <CancelReservation v-if="this.route === 'cancelreservation'" :fetchData="fetchData" :activeReservation="activeReservation" :changeRoute="changeRoute" />
      <Reservations v-if="this.route === 'reservations' && this.admin" :changeRoute="changeRoute" :changeActiveReservation="changeActiveReservation" :pastReservations="pastReservations" :reservations="reservations" :finished="finished" :cancelled="cancelled" />
      <AdminCreateReservation v-if="this.admin && this.route==='admincreatereservations'" :fetchData="fetchData" :user="user" :users="users"/>
      <Users v-if="this.admin && this.route==='users'" :users="users" :fetchData="fetchData"/>
    </div>
  </div>
</template>
