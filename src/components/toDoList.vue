<template>
    <div>
        <div class="cabecera">
            <h2>Lista de tareas</h2>
            <label for="tarea">Tarea: </label>
            <input type="text" v-model="nuevaTarea" v-on:keyup.enter="addTarea"><br><br>
            <label for="prioridad">Prioridad: </label>
            <select v-model="prioridad">
                <option disabled value="">-----</option>
                <option>1 <!--Alta--></option>
                <option>2 <!--Media--></option>
                <option>3 <!--Baja--></option>
            </select><br><br>
            <button v-bind:disabled="prioridad=='' || tarea==''" v-on:click="addTarea">Añadir tarea</button>
        </div>
        <transition-group name="efecto">
        <div class="tareas" v-for="(tarea, index) in listaTareas" :key="index" :class="{normal:!tarea.estado, completada:tarea.estado}">
            <ul>
                <li>
                    Tarea: {{tarea.nombre}}<br>
                    Prioridad: {{tarea.prioridad}}<br>
                    {{tarea.fecha}}<br>
                </li>
                <button @click="aumentar(index)">Aumentar prioridad</button>
                <button @click="disminuir(index)">Disminuir prioridad</button><br>
                <button @click="completar(index)"><img src="../assets/imagenes/check.png" id="check" alt="check"></button>
                <button @click="borrarTarea"><img src="../assets/imagenes/delete.png" id="delete" alt="delete"></button>
            </ul>
        </div>
        </transition-group>
        <div class="footer">
            <button @click="mostrarCompletadas">Tareas completadas</button>
            <button @click="mostrarIncompletas">Tareas incompletadas</button>
            <button @click="mostrarTodas">Mostrar todas</button><br>
            <button @click="borrarCompletadas">Borrar completadas</button>
            <button @click="borrarTodas">Eliminar todas</button>
            <h4>Tienes {{listaTareas.length}} tareas, de las cuales {{incompletos}} están pendientes</h4>
        </div>
    </div>
</template>

<script>
export default {
    name: 'todoList',
    components:{
        
    },
    data(){
        return{
            listaTareas: [],
            nuevaTarea: '',
            prioridad:'',
        }
    },
    methods:{
        addTarea(){
            if(this.nuevaTarea && this.prioridad){
                this.listaTareas.push({
                    nombre: this.nuevaTarea,
                    fecha: new Date().toLocaleString(),
                    prioridad: this.prioridad,
                    estado:false
                });
                this.nuevaTarea = '';
                this.prioridad = '';
                this.ordenar();
                this.updateLocalStorage()
            }
        },
        updateLocalStorage(){
            localStorage.all = JSON.stringify(this.listaTareas);
        },
        ordenar(){
            this.listaTareas = this.listaTareas.sort((a,b) => {
                if(a.prioridad < b.prioridad){
                    return -1;
                }else if(a.prioridad > b.prioridad){
                    return 1;
                }else{
                    return 0;
                }
            });
            this.updateLocalStorage();
        },
        aumentar(index){
            if(this.listaTareas[index].prioridad > 1){
                this.listaTareas[index].prioridad--;
                this.ordenar();
            }
        },
        disminuir(index){
            if(this.listaTareas[index].prioridad < 3){
                this.listaTareas[index].prioridad++;
                this.ordenar();
            }
        },
        completar(index){
            this.listaTareas[index].estado = !this.listaTareas[index].estado;
            this.updateLocalStorage();
        },

        borrarTarea(index){
            this.listaTareas.splice(index, 1);
            this.updateLocalStorage();
        },
        
       
        mostrarCompletadas(){
            this.listaTareas = JSON.parse(localStorage.all);
            let completas = [];
            this.listaTareas.forEach(tarea =>{
                if(tarea.estado){
                    completas.push(tarea);
                }
                this.listaTareas = completas;
            })
        },
        mostrarIncompletas(){
            this.listaTareas = JSON.parse(localStorage.all);
            let incompletas = [];
            this.listaTareas.forEach(tarea =>{
                if(!tarea.estado){
                    incompletas.push(tarea);
                }
                this.listaTareas = incompletas;
            })
        },
         borrarCompletadas(){
            let arr =[];
            this.listaTareas.forEach(tarea => {
                if(tarea.estado == false){
                    arr.push(tarea);
                }
            });
            this.listaTareas = arr;
            this.ordenar();
            this.updateLocalStorage();
        },

        mostrarTodas(){
            this.listaTareas = JSON.parse(localStorage.all);
        },
        borrarTodas(){
            this.listaTareas = [];
        },

    },
    computed:{
        incompletos(){
            let num_incompletos = this.listaTareas.filter(tarea => !tarea.estado).length;
            return num_incompletos;
        }
    },
    mounted(){
        if(localStorage.all)
            this.listaTareas = JSON.parse(localStorage.all);
    }
}
</script>

<style scoped>
.cabecera{
    margin: auto;
    background: violet;
    height: 155px;
    width: 60%;
    border-radius: 22px;
    opacity: 75%;
    color: white;
    margin-bottom: 10px;
    z-index: 2;
}

div ul{
    list-style: none;
}

div ul li #marcador{
    display: flex;
    width: 50px;
    height: 50px;
    border-radius: 50%;;
}

.normal{
    margin: auto;
    background: red;
    opacity: 70%;
    border-radius: 22px;
}

.completada{
    margin: auto;
    background: green;
    opacity: 70%;
    border-radius: 22px;
    text-decoration: line-through;
}

#check{
    height: 20px;
    width: 20px;
}

#delete{
    height: 20px;
    width: 20px;
}

.tareas{
    margin: auto;
    width: 60%;
    border-radius: 22px;
    opacity: 80%;
}

.footer{
    padding-top: 20px;
    margin: auto;
    background: yellow;
    height: 100px;
    width: 60%;
    border-radius: 22px;
    opacity: 80%;
}

/* Animaciones */
.efecto-enter-active{
    transition: all 1.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.efecto-leave-from,
.efecto-enter-to{
    transform: rotateX(-20px);
    opacity: 60%;
}

.efecto-leave-active {
    transition: all 1.5s cubic-bezier(1, 0.5, 0.8, 1);
  }
  
.efecto-enter-from,
.efecto-leave-to {
    transform: translateX(20px);
    opacity: 10%;
}
</style>