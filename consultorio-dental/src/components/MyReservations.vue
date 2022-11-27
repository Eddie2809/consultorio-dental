<script>
    export default{
        props: ['reservations','finished','cancelled','pastReservations','changeRoute','changeActiveReservation'],
        methods: {
            cancelReservation(reservation){
                this.changeActiveReservation(reservation)
                this.changeRoute('cancelreservation')
            }
        }
    }
</script>

<template>
    <div class="my-reservations">
        <img src="../assets/banner.png" alt="">
        <h1>Ver mis reservaciones</h1>
        <h2>Reservaciones pendientes</h2>
        <div class="table">
            <div class="names">
                <div class="id"><p>id</p></div>
                <div class="fecha"><p>Fecha</p></div>
                <div class="hora-inicio"><p>Hora de inicio</p></div>
                <div class="hora-fin"><p>Hora de fin</p></div>
                <div class="motivo"><p>Motivo</p></div>
                <div class="boton-cancelar"><p>Cancelar</p></div>
            </div>
            <div v-for="reservation in this.reservations" class="row-values">
                <div class="id"><p class="id">{{reservation.id}}</p></div>
                <div class="fecha"><p class="fecha">{{reservation.hora_inicio.substr(0,10)}}</p></div>
                <div class="hora-inicio"><p class="hora_inicio">{{reservation.hora_inicio.substr(11,5)}}h</p></div>
                <div class="hora-fin"><p class="hora_fin">{{reservation.hora_fin.substr(11,5)}}h</p></div>
                <div class="motivo"><p class="motivo_creacion">{{reservation.motivo_creacion}}</p></div>
                <div class="boton-cancelar"><button @click="this.cancelReservation(reservation)">Cancelar reservación</button></div>
            </div>
        </div>
        <h2>Reservaciones pasadas</h2>
        <div class="table">
            <div class="names">
                <div class="id"><p>id</p></div>
                <div class="fecha"><p>Fecha</p></div>
                <div class="hora-inicio"><p>Hora de inicio</p></div>
                <div class="hora-fin"><p>Hora de fin</p></div>
                <div class="motivo"><p>Motivo de cita</p></div>
                <div class="motivo-cancelar"><p>Motivo de cancelación</p></div>
            </div>
            <div v-for="reservation in this.pastReservations" :class="'row-values' + (reservation.cancelada === true? '':' cancelado')">
                <div class="id"><p>{{reservation.id}}</p></div>
                <div class="fecha"><p>{{reservation.hora_inicio.substr(0,10)}}</p></div>
                <div class="hora-inicio"><p>{{reservation.hora_inicio.substr(11,5)}}h</p></div>
                <div class="hora-fin"><p>{{reservation.hora_fin.substr(11,5)}}h</p></div>
                <div class="motivo"><p>{{reservation.motivo_creacion}}</p></div>
                <div class="motivo-cancelar"><p>{{reservation.motivo_cancelacion}}</p></div>
            </div>
        </div>
    </div>
</template>