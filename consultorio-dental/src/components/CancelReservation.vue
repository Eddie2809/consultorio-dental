<script>
    export default{
        props: ['activeReservation','fetchData','changeRoute'],
        data(){
            return{
                motivo: ''
            }
        },
        methods: {
            cancelReservation(){
                this.fetchData('cancelar-reservacion',{idReservacion: this.activeReservation.id, motivo: this.motivo}).then(res => {
                    alert('Reservación cancelada con éxito')
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
    <div class="cancel-reservation">
        <img src="../assets/banner.png" alt="banner">
        <h1>Cancelar reservación</h1>
        <h2>Reservación: {{this.activeReservation.id}}</h2>
        <div class="table">
            <div class="names">
                <div class="id"><p>id</p></div>
                <div class="fecha"><p>Fecha</p></div>
                <div class="hora-inicio"><p>Hora de inicio</p></div>
                <div class="hora-fin"><p>Hora de fin</p></div>
                <div class="motivo"><p>Motivo</p></div>
            </div>
            <div class="row-values">
                <div class="id"><p>{{this.activeReservation.id}}</p></div>
                <div class="fecha"><p>{{this.activeReservation.hora_inicio.substr(0,10)}}</p></div>
                <div class="hora-inicio"><p>{{this.activeReservation.hora_inicio.substr(11,5)}}</p></div>
                <div class="hora-fin"><p>{{this.activeReservation.hora_fin.substr(11,5)}}</p></div>
                <div class="motivo"><p>{{this.activeReservation.motivo_creacion}}</p></div>
            </div>
            <h3>Escribe el motivo de la cancelación (Opcional): </h3>
            <textarea v-model="motivo" name="" id="" cols="30" rows="10"></textarea>
            <button @click="cancelReservation()">Cancelar reservación</button>
        </div>
    </div>
</template>