<template>
  <header><img src="./assets/imagenes/lista-de-tareas.jpg"></header>
  <ToDoList></ToDoList>
</template>

<script>
import ToDoList from './components/toDoList.vue'

export default {
  name: 'App',
  components: {
    ToDoList
  }
}
</script>

<style>
body{
  text-align: center;
  background-image: url('./assets/imagenes/tareas.jpg');
}

header{
    height: 220px;
    width: 100%;
}

header img{
    height: 220px;
    width: 60%;
    border-radius: 20px;
}
</style>