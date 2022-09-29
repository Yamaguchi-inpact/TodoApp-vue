<template>
    <!--<img alt="Vue logo" src="@/assets/logo.png">-->
    <div class="category">
        <div v-if="loading" class="loading"> Loading...</div>

        <div v-if="post" class="content">
            <table border="1" class="categoryList">
                <thead>
                    <tr>
                        <th>Category</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="category in post" :key="category.CategoryId" @from-child="goDetail()" class="categoryItems">
                        <td>
                            <div class="category_content">
                                <div class="category_name">
                                    <div class="">{{category.CategoryName}}</div>
                                </div>
                                <div class="">
                                    <button @click="removeTodo(category.CategoryId)">削除</button> |
                                    <button @click="goDetail(category.CategoryId)">編集</button>
                                </div>
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

let url ="https://localhost:5001/api/category/";

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

.category {
    background: #abc7bd;
}
.content {
     text-align: center;
}
.categoryList {
    border-collapse: collapse;
    border: 3px solid black;

    width: 80%;
    margin: 0 auto;
    max-width: 520px;
}
.category_content {
    display: flex;
    flex-direction: column;
}
.categoryName {
    text-align: center;
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

 .categoryItems {
     border: 1px solid black;
     padding: 6px 20px;
 }
</style>