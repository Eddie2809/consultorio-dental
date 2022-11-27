<script>
    export default{
        props: ['fetchData','user','changeRoute'],
        data(){
            return {
                motive: '',
                day: 1,
                month: 1,
                year: 2022,
                hourStart: 0,
                minutesStart: 0,
                hourEnd: 0,
                minutesEnd: 0
            }
        },
        methods: {
            createReservation(){
                let today = new Date()
                let dayString = this.day < 10 ? '0' + this.day : this.day
                let monthString = this.month < 10 ? '0' + this.month : this.month
                let hourStartString = this.hourStart < 10 ? '0' + this.hourStart : this.hourStart
                let minutesStartString = this.minutesStart < 10 ? '0' + this.minutesStart : this.minutesStart
                let hourEndString = this.hourEnd < 10 ? '0' + this.hourEnd : this.hourEnd
                let minutesEndString = this.minutesEnd < 10 ? '0' + this.minutesEnd : this.minutesEnd

                let dayObjStart = new Date(this.year + '-' + monthString + '-' + dayString + 'T' + hourStartString + ':' + minutesStartString)
                let dayObjEnd = new Date(this.year + '-' + monthString + '-' + dayString + 'T' + hourEndString + ':' + minutesEndString)

                try{
                    console.log(dayObjEnd.toISOString())
                    console.log(dayObjStart.toISOString())
                }
                catch{
                    alert('Fecha inválida')
                    return
                }

                if(dayObjStart <= today || dayObjEnd <= today){
                    alert('Ingresa una fecha válida')
                    return
                }

                this.fetchData('crear-reservacion',{
                    horaInicio: dayObjStart.toISOString(),
                    horaFin: dayObjEnd.toISOString(),
                    motivoCreacion: this.motive,
                    idCliente: this.user.id,
                    idCreador: this.user.id
                })
                .then(res => {
                    alert('Reservación creada con éxito')
                    this.changeRoute('myreservations')
                })
                .catch(err => {
                    alert('Algo salió mal')
                })
            }
        }
    }
</script>

<template>
    <div class="make-reservations">
        <img src="../assets/banner.png" alt="banner">
        <h1>Hacer nueva reservación</h1>
        <p>Motivo: </p>
        <textarea v-model="this.motive" name="" id="" cols="30" rows="10"></textarea>
        <div class="fecha">
            <p>Fecha: <input v-model="this.day" type="number" name="" id=""> / <input v-model="this.month" type="number" name="" id=""> / <input v-model="this.year" type="number" name="" id=""></p>
        </div>
        <div class="hora">
            <p>Hora de inicio: <input v-model="this.hourStart" type="number" name="" id=""> : <input v-model="this.minutesStart" type="number" name="" id=""></p>
            <p>Hora de Fin: <input v-model="this.hourEnd" type="number" name="" id=""> : <input v-model="this.minutesEnd" type="number" name="" id=""></p>
        </div>
        <button @click="createReservation()">Realizar reservación</button>
    </div>
</template>
