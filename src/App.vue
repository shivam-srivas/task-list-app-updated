<!-- <script setup>
import TodoApp from './components/TodoApp.vue';

</script>

<template>
  <header>

    <div id="app">
      <TodoApp/>
    </div>
  </header>

  <main>
    <TodoApp />
  </main>
</template> -->


<template>
  <div id="app">
    <todo-app></todo-app>
   
  </div>
</template>

<script>
import TodoApp from './components/TodoApp.vue'

export default {
  name: 'App',
  components: {
    TodoApp
    
  }
}
</script>