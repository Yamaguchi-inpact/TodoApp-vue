<template>
  <div class="addTodo">
    <h1>Add Todo</h1>
    <div class="form">
      <p>タイトル</p>
      <input v-model="data.title" />
      <p>カテゴリー</p>
      <input type="number" v-model="data.categoryId" />
      <p>テキスト</p>
      <textarea v-model="data.text"></textarea>
      <button type="button" @click="postData()">送信</button>
    </div> 
  </div>
</template>
<script>
import axios from 'axios'
import router from '@/router'
import { reactive } from "vue";
let url = "https://localhost:5001/api/todoitems/";

  export default {
    setup() {
      const data = reactive({
          title: "",
          text: "",
          categoryId: 0,
      });
      const postData = () => {
          axios
          .post(url, {
            TodoTitle: data.title,
            TodoText: data.text,
            categoryId: data.categoryId,
          })
          .then((res) => {
            console.log(res);
            this.posts = res.data.posts;
          })
          .catch((err) => {
            console.log(err);
          })
          // .then(()=>{
          //   goList();
          // });
      };
      const goList = () => {
        router.push('/list');
      };
    return { data, postData, goList };
  },
};
</script>
<style scoped>
.addTodo{
  text-align: center;
}
.form {
  display: flex;
  flex-direction: column;
  
  width: 80%;
  margin: 0 auto;
  max-width: 500px;
}
textarea {
  resize: none;
  height: 10em;
}
    
</style>