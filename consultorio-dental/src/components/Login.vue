<script>
    export default{
        data(){
            return{
                email: '',
                password: '',
                error: false,
                remindme: false
            }
        },
        props: ['fetchData','fetchGet','changeRoute','loadUser'],
        methods: {
            submitData(){
                this.fetchData('verificar-credenciales',{correo: this.email, contrasena: this.password}).then(res => {
                    if(res.length){
                        this.loadUser(res)

                        if(this.remindme){
                            this.fetchData('generar-sid',{userId: res[0].id}).then(sid => {
                                let expiracion = new Date()
                                expiracion.setTime(expiracion.getTime() + (1000*60*60*24*30))
                                let nuevaCookie = 'SID=' + sid + ';expires=' + expiracion + ';'
                                document.cookie = nuevaCookie
                            })
                        }
                    }
                    else{
                        this.error = true
                    }
                })
            },
            alerta(){
                alert('Contacte al administrador de la página')
            }
        }
    }
</script>

<template>
    <div class="Login">
        <div class="card">
            <img src="../assets/logo-login.jpeg" alt="logo">
            <p class="h1">Bienvenido</p>
            <p v-if="this.error" class="error">Credenciales incorrectas</p>
            <input v-model="email" class="text first-text" type="text" placeholder="Correo">
            <input v-model="password" class="text second-text" type="password" placeholder="Contraseña">
            <div class="lower-options">
                <div class="left">
                    <input v-model="remindme" type="checkbox" name="" id="">
                    <p>Recordarme</p>
                </div>
                <p class="right pointer" @click="this.alerta">Olvidé mi contraseña</p>
            </div>
            <button @click="this.submitData">Iniciar sesión</button>
            <p class="signup-link">¿No tienes cuenta? <span @click="this.changeRoute('signup')" class="pointer"><u>Crear cuenta</u></span></p>
        </div>
    </div>
</template>