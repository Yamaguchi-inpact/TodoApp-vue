<template>
  <div class="addCategory">
    <h1>Add Category</h1>
    <div class="form">
      <p>Category</p>
      <input v-model="data.categoryName" />
      <span>選択オプション: {{data.categoryName}}</span>
      <p>Color</p>
      <div class="colorPick">
        <input type="radio" id="color1" value= 0 v-model="data.colorId" />
        <label for="color1">オプション1</label>
        <br>
        <input type="radio" id="color2" value= 1 v-model="data.colorId" />
        <label for="color2">オプション2</label>
        <br>
        <input type="radio" id="color3" value= 2 v-model="data.colorId" />
        <label for="color3">オプション3</label>
        <br>
        <span>選択オプション: {{data.colorId}}</span>
      </div>
      <p>State</p>
      <div class="statePick">
        <input type="radio" id="state1" value= 0 v-model="data.stateId" />
        <label for="state1">オプション1</label> |
        <input type="radio" id="state2" value= 1 v-model="data.stateId" />
        <label for="state2">オプション2</label> |
        <input type="radio" id="state3" value= 2 v-model="data.stateId" />
        <label for="state3">オプション3</label>
        <br>
        <span>選択オプション: {{data.stateId}}</span>
      </div>
      <button type="button" @click="postData()">送信</button>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import router from '@/router'
import { reactive } from "vue";
let url = "https://localhost:5001/api/category/";

  export default {
    setup() {
      const data = reactive({
            categoryName: "",
            colorId: 0,
            stateId: 0,
      });
      const postData = () => {
          axios
          .post(url, {
            CategoryName: data.categoryName,
            ColorId: data.colorId,
            StateId: data.stateId,
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
.addCategory{
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
.colorPick .statePick {
  display: flex; 
}
    
</style>