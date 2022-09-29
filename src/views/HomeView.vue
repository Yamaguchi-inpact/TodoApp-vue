<template>
    <div class="home">
      <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
      <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
        <TodoList />
        <div class="home_wrapper_col">
          <div class="form_button">
            <div class="button_wrap">
              <p>Add Todo</p>
              <button class="button" @click="openModal(0)">Add</button>
            </div>
            <div class="overlay" v-show="showContent[0]">
              <div class="content">
                <AddTodo />
                <button v-on:click="closeModal">Close</button>
              </div>
            </div>
          </div>
          <div class="form_button">
            <div class="button_wrap">
              <p>Add Category</p>
              <button class="button" @click="openModal(1)">Add</button>
            </div>
            <div class="overlay" v-show="showContent[1]">
              <div class="content">
                <AddCategory/>
                <button class="button" v-on:click="closeModal">Close</button>
              </div>
            </div>
          </div>
          <CategoryList/>
        </div>
      </div>
</template>

<script>
// @ is an alias to /src
import TodoList from '@/components/TodoList.vue'
import AddTodo from '../components/AddTodo.vue'
import AddCategory from '../components/AddCategory.vue'
import CategoryList from '@/components/CategoryList.vue';
import { ref } from "vue";

export default {
  name: 'HomeView',
  components: {
    TodoList,
    AddTodo,
    AddCategory,
    CategoryList
},
  setup() {
    const showContent = ref([
      false,
      false
    ]);

    const openModal = (i) => {
      console.log('click');
      showContent.value[i] = true;
    };

    const closeModal = () => {
      showContent.value = [false,false];
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
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  .home_wrapper_col {
    display: flex;
    flex-direction: column;
    width: 25%;
  }

  .form_button {
    height:15%;
    background: #c5e5da;
  }
  .button_wrap {
    text-align: center;
  }
  
  .button {
    display: block;
    margin: auto;
    text-decoration: none;
    padding: 0.2em 3em;
    margin-top: 0.5em;
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