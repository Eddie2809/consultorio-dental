<script>
  import Login from './components/Login.vue'
  import Signup from './components/Signup.vue'
  import Home from './components/Home.vue'
  import Navbar from './components/Navbar.vue'
  import MyReservations from './components/MyReservations.vue'
  import MakeReservation from './components/MakeReservation.vue'
  import CancelReservation from './components/CancelReservation.vue'

  export default{
    components: {
      Login,
      Signup,
      Home,
      Navbar,
      MyReservations,
      MakeReservation,
      CancelReservation
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
        activeReservation: {} 
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
      changeRoute(newRoute){
          this.route = newRoute
      },
      changeActiveReservation(newactive){
        this.activeReservation = newactive
      },
      loadUser(user){
        this.loggedIn = true
        this.user = user[0]

        this.fetchData('obtener-reservaciones-pendientes',{idUsuario: this.user.id}).then(pendientes=> {
          this.fetchData('obtener-reservaciones-canceladas',{idUsuario: this.user.id}).then(canceladas => {
            this.fetchData('obtener-reservaciones-finalizadas',{idUsuario: this.user.id}).then(finalizadas => {
              this.reservations = pendientes
              this.cancelled = canceladas
              this.finished = finalizadas

              this.pastReservations = this.cancelled.concat(this.finished)
            })
          })
        })
        this.changeRoute('myreservations')
      },
      logout(){
        this.loggedIn = false
        this.user = null

        this.changeRoute('home')
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
      <Navbar :route="route" :changeRoute="this.changeRoute" :logout="this.logout" />
      <MyReservations v-if="this.route === 'myreservations'" :changeRoute="changeRoute" :changeActiveReservation="changeActiveReservation" :pastReservations="pastReservations" :reservations="reservations" :finished="finished" :cancelled="cancelled" />
      <MakeReservation v-if="this.route === 'makereservation'" :changeRoute="changeRoute" :user="this.user" :fetchData="fetchData"/>
      <CancelReservation v-if="this.route === 'cancelreservation'" :fetchData="fetchData" :activeReservation="activeReservation" :changeRoute="changeRoute" />
    </div>
  </div>
</template>
