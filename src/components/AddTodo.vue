<template>
  <div class="addTodo">
    <h1>Add Todo</h1>
    <div class="form">
      <p>タイトル</p>
      <input v-model="data.title" />
      <p>カテゴリー</p>
      <select v-model="data.categoryId">
        <option disabled value= null>Categories</option>
        <option v-for="cate in data.json_data" 
          :value="cate.CategoryId" 
          :key="cate.CategoryId">
        {{ cate.CategoryName }}
        </option>
      </select>
      <p>テキスト</p>
      <textarea v-model="data.text"></textarea>
      <button type="button" @click="postData()">送信</button>
    </div> 
  </div>
</template>
<script>
import axios from 'axios'
import router from '@/router'
import { reactive, onMounted } from "vue";
let url = "https://localhost:5001/api";


  export default {
    setup() {
      const data = reactive({
        json_data : null,
        title: "",
        text: "",
        categoryId : null,
      });
      const getData = async () => {
        await axios
        .get(`${url}/category`)
        .then((res) => {
          console.log("category.data",res.data);
          data.json_data = res.data;
        })
        .catch((err) => {
            console.log(err);
        })
        .then(()=>{
          initData();
        });
      };
      const initData = () => {
        data.categoryId = data.json_data ? data.json_data[0].CategoryId : null ;
      };
      const postData = () => {
          axios
          .post(`${url}/todoitems`, {
            TodoTitle: data.title,
            TodoText: data.text,
            CategoryId: data.categoryId,
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
      onMounted(() => {
        getData();
      });
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