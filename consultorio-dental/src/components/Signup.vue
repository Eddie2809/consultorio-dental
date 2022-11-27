<script>
    export default{
        props: ['fetchGet', 'fetchData', 'changeRoute'],
        data(){
            return{
                name: '',
                lastname: '',
                email: '',
                password: '',
                secondPassword: '',
                phone: ''
            }
        },
        methods: {
            submitData(){
                if(this.name === '' || this.lastname === '' || this.email === '' || this.password === '' || this.secondPassword === ''){
                    alert('Rellene todos los campos')
                    return
                }
                if(this.password !== this.secondPassword){
                    alert('Las contraseñas no coinciden')
                }
                
                this.fetchData('registrar-usuario',{
                    nombre: this.name,
                    apellido: this.lastname,
                    contrasena: this.password,
                    correo: this.email,
                    telefono: this.phone,
                    tipo_de_usuario: 3
                }).then(res => {
                    if(res !== 'Algo salió mal'){
                        alert('Éxito')
                        this.changeRoute('login')
                    }
                })
            }
        }
    }
</script>

<template>
    <div class="signup">
        <div class="card">
            <img src="../assets/logo-signup.jpeg" alt="logo"><br>
            <h1>Crear nueva cuenta</h1><br>
            <div>
                <p>Nombre</p><br>
                <input v-model="this.name" type="text"><br>
            </div>
            <div>
                <p>Apellido</p><br>
                <input v-model="this.lastname" type="text"><br>
            </div>
            <div>
                <p>Correo</p><br>
                <input v-model="this.email" type="text"><br>
            </div>
            <div>
                <p>Telefono</p><br>
                <input v-model="this.phone" type="text">
            </div>
            <div>
                <p>Contraseña</p><br>
                <input v-model="this.password" type="password" name="" id=""><br>
            </div>
            <div>
                <p>Repite la contraseña</p><br>
                <input v-model="this.secondPassword" type="password" name="" id=""><br>
            </div>
            <button @click="this.submitData">Crear cuenta</button><br>
            <p class="login" @click="this.changeRoute('login')" ><u>Iniciar sesión</u></p><br>
        </div>
    </div>
</template>