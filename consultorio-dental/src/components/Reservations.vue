<script>
    export default{
        props: ['reservations','finished','cancelled','pastReservations','changeRoute','changeActiveReservation','fetchData'],
        methods: {
            cancelReservation(reservation){
                this.changeActiveReservation(reservation)
                this.changeRoute('cancelreservation')
            },
            finishReservation(reservation){
                this.fetchData('finalizar-reservacion',{idReservacion: reservation.id}).then(res => {
                    alert('Tarea realizada con éxito')
                    location.reload()
                })
                .catch(err => {
                    alert('Algo salió mal')
                })
            },
            evaluateReservation(reservation){
                let today = new Date()
                let reservationDay = new Date(reservation.hora_fin)

                if(today > reservationDay){
                    return ' yellow'
                }
                else{
                    return ''
                }
            }
        }
    }
</script>

<template>
    <div class="reservations">
        <img src="../assets/banner.png" alt="">
        <h1>Ver mis reservaciones</h1>
        <h2>Reservaciones pendientes</h2>
        <div class="table">
            <div class="names">
                <div class="id"><p>id</p></div>
                <div class="usuario">Usuario</div>
                <div class="fecha"><p>Fecha</p></div>
                <div class="hora-inicio"><p>Hora de inicio</p></div>
                <div class="hora-fin"><p>Hora de fin</p></div>
                <div class="motivo"><p>Motivo</p></div>
                <div class="boton-cancelar"><p>Opciones</p></div>
            </div>
            <div v-for="reservation in this.reservations" :class="'row-values' + this.evaluateReservation(reservation)">
                <div class="id"><p class="id">{{reservation.id}}</p></div>
                <div class="usuario"><p>{{reservation.nombre + ' ' + reservation.apellido}}</p></div>
                <div class="fecha"><p class="fecha">{{reservation.hora_inicio.substr(0,10)}}</p></div>
                <div class="hora-inicio"><p class="hora_inicio">{{reservation.hora_inicio.substr(11,5)}}h</p></div>
                <div class="hora-fin"><p class="hora_fin">{{reservation.hora_fin.substr(11,5)}}h</p></div>
                <div class="motivo"><p class="motivo_creacion">{{reservation.motivo_creacion}}</p></div>
                <div class="boton-finalizar"><button @click="this.finishReservation(reservation)">Finalizar</button></div>
                <div class="boton-cancelar"><button @click="this.cancelReservation(reservation)">Cancelar</button></div>
            </div>
        </div>
        <h2>Reservaciones pasadas</h2>
        <div class="table">
            <div class="names">
                <div class="id"><p>id</p></div>
                <div class="usuario"><p>Usuario</p></div>
                <div class="fecha"><p>Fecha</p></div>
                <div class="hora-inicio"><p>Hora de inicio</p></div>
                <div class="hora-fin"><p>Hora de fin</p></div>
                <div class="motivo2"><p>Motivo de cita</p></div>
                <div class="motivo2"><p>Motivo de cancelación</p></div>
            </div>
            <div v-for="reservation in this.pastReservations" :class="'row-values' + (reservation.cancelada === true? '':' cancelado')">
                <div class="id"><p>{{reservation.id}}</p></div>
                <div class="usuario"><p>{{reservation.nombre + ' ' + reservation.apellido}}</p></div>
                <div class="fecha"><p>{{reservation.hora_inicio.substr(0,10)}}</p></div>
                <div class="hora-inicio"><p>{{reservation.hora_inicio.substr(11,5)}}h</p></div>
                <div class="hora-fin"><p>{{reservation.hora_fin.substr(11,5)}}h</p></div>
                <div class="motivo2"><p>{{reservation.motivo_creacion}}</p></div>
                <div class="motivo2"><p>{{reservation.motivo_cancelacion}}</p></div>
            </div>
        </div>
    </div>
</template>