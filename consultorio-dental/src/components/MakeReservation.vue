<script>
    export default{
        props: ['fetchData','user','changeRoute'],
        data(){
            return {
                motive: '',
                day: 1,
                month: 1,
                year: 2022,
                hourStart: 9,
                minutesStart: 0,
                hourEnd: 9,
                minutesEnd: 30
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

                if(this.motive === ''){
                    alert('Ingresa el motivo de la cita')
                    return
                }

                if(dayObjStart >= dayObjEnd){
                    alert('La hora de inicio tiene que ser antes de la hora de fin')
                    return
                }

                if(this.hourStart < 9){
                    alert('El horario del consultorio es de 09:00 - 20:00')
                }

                if(this.hourEnd >= 20 && this.minutesEnd > 0){
                    alert('El horario del consultorio es de 09:00 - 20:00')
                }
                let minutes = ((dayObjEnd - dayObjStart) / 1000 / 60)

                if(minutes > 90 || minutes < 20){
                    alert('La cita tiene que durar entre 20 y 90 minutos')
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
                    location.reload()
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
            <p>Hora de inicio: <input min="9" max="19" v-model="this.hourStart" type="number" name="" id=""> : <input min="0" max="59" v-model="this.minutesStart" type="number" name="" id=""></p>
            <p>Hora de Fin: <input min="9" max="20" v-model="this.hourEnd" type="number" name="" id=""> : <input v-model="this.minutesEnd" min="0" max="59" type="number" name="" id=""></p>
        </div>
        <button @click="createReservation()">Realizar reservación</button>
    </div>
</template>
