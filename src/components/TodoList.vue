<template>
    <!--<img alt="Vue logo" src="@/assets/logo.png">-->
    <div class="post">
        <div class="about">
            <h1>TodoList</h1>
        </div>
        <div v-if="loading" class="loading">
            Loading... Please refresh once the ASP.NET backend has started. See <a href="https://aka.ms/jspsintegrationvue">https://aka.ms/jspsintegrationvue</a> for more details.
        </div>

        <div v-if="post" class="content">
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
                    <tr v-for="todo in post" :key="todo.TodoItemId" @from-child="goDetail()" class="todoitems">
                        <td><div class="ellipsis">{{ todo ? todo.TodoTitle : "---" }}</div></td>
                        <td><div class="ellipsis">{{ todo ? todo.TodoText : "---" }}</div></td>
                        <td><div class="">{{ todo ? todo.Created : "---" }}</div></td>
                        <td><div class="ellipsis">{{ todo ? todo.Modified : "---" }}</div></td>
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
    import { defineComponent } from 'vue';

let url ="https://localhost:5001/api/todoitems/";

export default defineComponent({
    data() {
        return {
            loading: false,
            post: null,
        };
    },
    created() {
        // fetch the data when the view is created and the data is
        // already being observed
        this.fetchData();
    },
    watch: {
        // call again the method if the route changes
        '$route': 'fetchData'
    },
    methods: {
        fetchData: function() {
            this.post = null;
            this.loading = true;

            fetch(url)
                .then(r => r.json())
                .then(json => {
                    this.post = json;
                    this.loading = false;
                    console.log(json);
                    return;
                });
        },
        removeTodo: function(id) {
            axios.delete(url + id)
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
                this.fetchData();
            });
        },
        goDetail: function(id) {
            let i = id;
            this.$router.push({name:'detail', params: {chosenId: i}})
            .catch(()=>{});
        }
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