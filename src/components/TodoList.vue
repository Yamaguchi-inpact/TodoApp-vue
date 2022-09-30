<template lang="html">
    <!--<img alt="Vue logo" src="@/assets/logo.png">-->
    <div class="post">
        <div class="about">
            <h1>TodoList</h1>
        </div>
        <div v-if="data.loading" class="loading">
            Loading... Please refresh once the ASP.NET backend has started. See <a href="https://aka.ms/jspsintegrationvue">https://aka.ms/jspsintegrationvue</a> for more details.
        </div>

        <div v-if="data.json_data" class="content">
            <table border="1" class="todo">
                <thead>
                    <tr>
                        <th>TITLE</th>
                        <th>TEXT</th>
                        <th>CREATED</th>
                        <th>MODIFIRED</th>
                        <th>ISCOMPLETE</th>
                        <th>EDIT</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="todo in reverseItems" :key="todo.TodoItemId" class="todoitems">
                        <td><div class="ellipsis">{{ todo ? todo.TodoTitle : "---" }}</div></td>
                        <td><div class="ellipsis">{{ todo ? todo.TodoText : "---" }}</div></td>
                        <td><div class="ellipsis">{{ todo ? todo.Created : "---" }}</div></td>
                        <td><div class="">{{ todo ? todo.Modified : "---" }}</div></td>
                        <td>{{ todo ? todo.IsComplete : "---" }}</td>
                        <td>
                            <div class="todoitems_edit">
                                <button @click="removeTodo(todo.TodoItemId)">削除</button> |
                                <button @click="goDetail(todo.TodoItemId)">編集</button>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

    </div>
</template>
<script lang="js">
    import axios from 'axios';
    import router from '@/router'
    import { defineComponent, reactive, computed, onMounted } from 'vue';

    
    let url ="https://localhost:5001/api";

export default defineComponent({
    setup() {
        const data = reactive ({
            loading: false,
            json_data: null
        });
        const getData = async () => {
            data.loading = true;
            data.json_data = null;

            await axios
            .get(`${url}/todoitems`)
            .then((r) => {
                console.log("r.data",r.data);
                data.json_data = r.data;
            })
            .then(() => {
                data.loading = false;
            });
        };
        const removeTodo = ((id) =>{
            axios.delete(`${url}/todoitems/${id}`)
            .then((r) => {
                // handle success
                console.log(r);
            })
            .catch((e) => {
                // handle error
                console.log(e);
            })
            .then(()=>{
                getData();
            });
        });
        const goDetail = (id) => {
            let i = id;
            router.push({name:'detail', params: {chosenId: i}})
        };
        let reverseItems = computed(() => {
        // 配列の要素順番を逆順にする
              return  data.json_data.slice().reverse()
        });
        onMounted(() => {
        getData();
      });
    return { data, removeTodo, goDetail, reverseItems };
    },
});
</script>
<style scoped>

.post {
    background: #abc7bd;
}
.content {
     text-align: center;
}
.todo {
    border-collapse: collapse;
    border: 3px solid black;

    width: 80%;
    margin: 0 auto;
    max-width: 520px;
}
.todoitems_edit {
    display: flex;
    align-items: center;
}
.ellipsis {
    max-width: 120px;
    padding-left: 4px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
button {
    min-width: 3em;
}

th {
    background-color: #5cb389;
}

tr {
    background-color: #c5e5da;
}

tr:nth-child(even) {
    background-color: #acd6cc;
}

 .todoitems {
     border: 1px solid black;
     padding: 6px 20px;
 }
</style>