<template>
    <!-- <nav>
        <router-link to="/">Home</router-link>
    </nav> -->
    <div class="app">
        <TodoHeader/>
        <router-view />
        <TodoFooter/>
    </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader.vue";
import TodoFooter from "./components/TodoFooter.vue";
export default {
    components: { TodoHeader, TodoFooter }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.app {
    display: flex;
    flex-direction: column;
}
</style>
