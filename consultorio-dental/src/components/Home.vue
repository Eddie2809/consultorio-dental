<script>
    export default{
        props: ['changeRoute']
    }
</script>

<template>
    <div class="Home">
        <img src="../assets/logo.jpeg" alt="">
        <div class="bottom">
            <button @click="this.changeRoute('login')">Iniciar sesión</button>
            <button @click="this.changeRoute('signup')">Crear nueva cuenta</button>
        </div>
    </div>
</template>