<template>
  <div class="base">
    <nav>
      <h1>Minhas tarefas do dia</h1>
    </nav>
    <tarefas />
  </div>
</template>

<script>
import Tarefas from './components/tarefas.vue'


export default {
  name: 'App',
  components: {
    Tarefas
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
</style>
<style scoped>
  nav{
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    padding-top: 10px;
    color: #5b7389;
  }
  .base{
    background-color: rgb(15,15,15);
    min-height: 100vh;
  }
</style>
