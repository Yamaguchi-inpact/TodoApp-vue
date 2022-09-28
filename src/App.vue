<template>
    <!-- <nav>
        <router-link to="/">Home</router-link>
    </nav> -->
    <div class="wrapper">
        <TodoHeader/>
        <router-view />
    </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader.vue";
export default {
    components: { TodoHeader }
}
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
.wrapper {
    display: flex;
    flex-direction: column;
}
</style>
