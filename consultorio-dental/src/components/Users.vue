<script>
    export default{
        props: ['users','fetchData'],
        methods: {
            cambiarRol(userId,rol){
                if(userId === 1){
                    alert('No puedes hacer eso')
                    return
                }
                if(rol === 'administrador'){
                    this.fetchData('cambiar-rol',{nuevoRol: 3, idUsuario: userId}).then(res => {
                        alert('Operación realizada con éxito')
                        location.reload()
                    })
                }
                else{
                    this.fetchData('cambiar-rol',{nuevoRol: 1, idUsuario: userId}).then(res => {
                        alert('Operación realizada con éxito')
                        location.reload()
                    })
                }
            }
        }
    }
</script>

<template>
    <div class="users">
        <h1>Usuarios registrados</h1>
        <div class="table">
            <div class="names">
                <div class="id"><p>id</p></div>
                <div class="usuario"><p>Usuario</p></div>
                <div class="correo"><p>Correo</p></div>
                <div class="telefono"><p>Teléfono</p></div>
                <div class="tipo"><p>Tipo de usuario</p></div>
                <div class="cambiar-rol">Cambiar rol</div>
            </div>
            <div v-for="user in this.users" :class="'row-values'">
                <div class="id"><p>{{user.id}}</p></div>
                <div class="usuario"><p>{{user.nombre + ' ' + user.apellido}}</p></div>
                <div class="correo"><p>{{user.correo}}</p></div>
                <div class="telefono"><p>{{user.telefono}}</p></div>
                <div class="tipo"><p>{{user.tipo_de_usuario}}</p></div>
                <div class="cambiar-rol"><button @click="this.cambiarRol(user.id,user.tipo_de_usuario)" :class="(user.tipo_de_usuario === 'administrador'? 'red':'green')">{{user.tipo_de_usuario === 'cliente'? 'Hacer admin':'Quitar admin'}}</button></div>
            </div>
        </div>
    </div>
</template>