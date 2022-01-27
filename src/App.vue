<template>
  <cabecera></cabecera>
  <ToDoList></ToDoList>
</template>

<script>
import ToDoList from './components/toDoList.vue'
import cabecera from './components/header.vue'

export default {
  name: 'App',
  components: {
    ToDoList,cabecera
  }
}
</script>

<style>
body{
  text-align: center;
}
</style>