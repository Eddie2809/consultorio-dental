<script>
    export default {
        props: ['changeRoute','logout','route','admin']
    }
</script>

<template>
    <div class="navbar">
        <img src="../assets/logopng.png" alt="">
        <div class="right">
            <div v-if="!this.admin" :class="this.route==='myreservations'? 'block selected': 'block'" @click="changeRoute('myreservations')" ><p>Ver mis reservaciones</p></div>
            <div v-if="!this.admin" :class="this.route==='makereservation'? 'block selected': 'block'" @click="changeRoute('makereservation')" ><p>Hacer reservación</p></div>
            <div v-if="this.admin" :class="this.route==='reservations'? 'block selected': 'block'" @click="changeRoute('reservations')" ><p>Ver reservaciones</p></div>
            <div v-if="this.admin" :class="this.route==='admincreatereservations'? 'block selected': 'block'" @click="changeRoute('admincreatereservations')" ><p>Crear una reservación</p></div>
            <div v-if="this.admin" :class="this.route==='users'? 'block selected': 'block'" @click="changeRoute('users')" ><p>Ver usuarios</p></div>
            <div class="block" @click="logout()" ><p>Cerrar sesión</p></div>
        </div>
    </div>
</template>