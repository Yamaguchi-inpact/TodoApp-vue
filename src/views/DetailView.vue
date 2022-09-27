<template>
  <div class="about">
    <h1>Todo Detail</h1>
  </div>
  <section class="alert alert-primary">
    <h1>Axios</h1>
    <p>{{data}}</p>
    <table class="table table-light table-striped">
      <tbody class="text-start">
        <tr>
          <th style="width: 200px">Title</th>
          <td><input v-model="data.title" ></td>
        </tr>
        <tr>
          <th>Text</th>
          <td><textarea v-model="data.text" ></textarea></td>
        </tr>
        <tr>
          <th>Created</th>
          <td>{{ data.created }}</td>
        </tr>
        <tr>
          <th>Modified</th>
          <td>{{ data.modified }}</td>
        </tr>
        <tr>
          <th>IsComplete</th>
          <td><input type="checkbox" v-model="data.isComplete" /></td>
        </tr>
      </tbody>
    </table>
    <button @click="putData(chosenId)">更新</button>
  </section>
</template>

<script lang="js">
  import router from "@/router";
  import axios from "axios";
  import { reactive, onMounted } from "vue";
  
  let url = "https://localhost:5001/api/todoitems/";
  
  export default {
    props: {
      chosenId: {
        type: Number,
        default: 0
      }
    },
    setup(props) {
      const data = reactive({
        json_data: null,
        title: '',
        text: '',
        isComplete: false,
        created: '',
        modified: '',
      });
      const goList = () => {
        router.push('/list')
      };
      const getData = async (id) => {
        await axios
        .get(url + id)
        .then((res) => {
          console.log("result.data",res.data);
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
        data.title = data.json_data ?  data.json_data[0].TodoTitle : '---';
        data.text = data.json_data ? data.json_data[0].TodoText : '---';
        data.isComplete = data.json_data ? data.json_data[0].IsComplete : '---';
        data.created = data.json_data ? data.json_data[0].Created : "---";
        data.modified = data.json_data ? data.json_data[0].Modified : "---";
      };
      const putData = (id) => {
        axios.put(url + id,{
          TodoTitle : data.title,
          TodoText : data.text,
          IsComplete : data.isComplete,
        })
        .then(function (response) {
          // handle success
          console.log(response);
        })
        .catch(function (error) {
          // handle error
          console.log(error);
        })
        .finally(function () {
          // always executed
        })
        .then(()=>{
          goList();
        });
      };
      onMounted(() => {
        let i = props.chosenId;
        getData(i);
      });
      return { data, getData, putData, goList };
    },
  };
  </script> 
  <Style></Style>