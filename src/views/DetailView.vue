<template>
    <!--<img alt="Vue logo" src="@/assets/logo.png">-->
    <div class="about">
        <h1>Todo Detail</h1>
    </div>
  <section class="alert alert-primary">
    <h1>{{ data.titleName }}</h1>
    <p>{{ data.message }}</p>
    <p>{{ data.title }}</p>
    <p>{{ data.text }}</p>
    <p>{{ data.isComplete }}</p>
    <p>ID:{{ chosenId }}</p>
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
          <td>{{ data.json_data ? data.json_data.created : "-" }}</td>
        </tr>
        <tr>
          <th>Modified</th>
          <td>{{ data.json_data ? data.json_data.modified : "-" }}</td>
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
      titleName: "Axios",
      message: "This is axios sample.",
      json_data: null,

      // id: 1, //id番号
      title: '',
      text: '',
      isComplete: false,
    });
    const goList = () => {
      router.push('/list')
    };
    const getData = (id) => {
      axios.get(url + id).then((result) => {
        console.log(result.data);
        data.json_data = result.data;
      })
      .then(()=>{
        initData();
      });
    };
    const initData = () => {
      data.title = data.json_data ? data.json_data.title : '---';
      data.text = data.json_data ? data.json_data.text : '---';
      data.isComplete = data.json_data ? data.json_data.isComplete : '---';
    };
    const putData = (id) => {
      axios.put(url + id,{
        id: props.chosenId,
        title: data.title,
        text : data.text,
        isComplete: data.isComplete,
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
      let i = props.chosenId
      getData(i);
    });
    return { data, getData, putData, goList };
  },
};
</script> 
<Style></Style>
