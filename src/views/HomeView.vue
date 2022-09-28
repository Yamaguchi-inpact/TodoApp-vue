<template>
    <div class="home">
      <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
      <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
      <div class="wrapper">
        <TodoList />
        <div class="app">
          <div class="button_wrap">
            <p>Add Todo</p>
            <button class="button" v-on:click="openModal">Add</button>
          </div>
          <div class="overlay" v-show="showContent">
            <div class="content">
              <AddTodo />
              <button v-on:click="closeModal">Close</button>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
// @ is an alias to /src
import TodoList from '@/components/TodoList.vue'
import AddTodo from '../components/AddTodo.vue'
import { ref } from "vue";

export default {
  name: 'HomeView',
  components: {
    TodoList,
    AddTodo,
  },
  setup() {
    const showContent = ref(false);

    const openModal = () => {
      console.log('click');
      showContent.value = true;
    };

    const closeModal = () => {
      showContent.value = false;
    };

    return {
      showContent,
      openModal,
      closeModal,
    };
  },
};
</script>

<style scoped>
  .home {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
  }
  .wrapper {
    display: flex;
    justify-content: center;
  }

  .app {
    display: block;
    width: 25%;
    background: #c5e5da;
    text-align: center;
  }

  .button {
    display: block;
    margin: auto;
    text-decoration: none;
    padding: 0.2em 3em;
  }
  .button_wrap {
    height: 25%;
  }

  .overlay {

      z-index: 1;

      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);

      display: flex;
      align-items: center;
      justify-content: center;
    }

    .content {
      z-index: 2;
      width: 50%;
      padding: 1em;
      background: #fff;
    }
  </style>