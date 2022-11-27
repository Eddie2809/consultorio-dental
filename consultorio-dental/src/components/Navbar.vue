<script>
    export default {
        props: ['changeRoute','logout','route']
    }
</script>

<template>
    <div class="navbar">
        <img src="../assets/logopng.png" alt="">
        <div class="right">
            <div :class="this.route==='myreservations'? 'block selected': 'block'" @click="changeRoute('myreservations')" ><p>Ver mis reservaciones</p></div>
            <div :class="this.route==='makereservation'? 'block selected': 'block'" @click="changeRoute('makereservation')" ><p>Hacer reservación</p></div>
            <div class="block" @click="logout()" ><p>Cerrar sesión</p></div>
        </div>
    </div>
</template>