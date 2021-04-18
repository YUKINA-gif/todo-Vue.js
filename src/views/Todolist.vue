<template>
  <div class="todolist">
    <h1>Todo List</h1>

    <div class="todoCreate flex">
      <input type="text" class="todo-input" v-model="todoItem">
      <input type="button" value="追加" class="button create-button" @click="createTodo">
    </div>

    <div v-for="(todo, index) in todos" :key="index" class="flex todolists">
      <input type="text" v-model="todo.todo" class="listItem">
      <div class="todo-item-buttons">
        <input type="button" value="更新" class="button update-button" @click="updateTodo(index)">
        <input type="button" value="削除" class="button delete-button" @click="deleteTodo(index)">
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      todos:[],
      todoItem:"",
    }
  },
  methods: {
    todoDisplay(){
      axios
      .get("https://lit-earth-49630.herokuapp.com/api/todo")
      .then((response) => {
        this.todos = response.data.data;
      });
    },
    createTodo(){
      axios
      .post("https://lit-earth-49630.herokuapp.com/api/todo",{
        todo:this.todoItem
      })
      .then((response) => {
        console.log(response);
        this.todoDisplay();
        this.todoItem = "";
      })
    },
    updateTodo(index){
      axios
      .put("https://lit-earth-49630.herokuapp.com/api/todo/" + this.todos[index].id,{
        todo:this.todos[index].todo
      })
      .then((response) => {
        console.log(response);
      })
    },
    deleteTodo(index){
      axios
      .delete("https://lit-earth-49630.herokuapp.com/api/todo/" + this.todos[index].id)
      .then((response) => {
        console.log(response);
        this.todoDisplay();
      })
    },
  },
  created() {
    this.todoDisplay()
  }
}
</script>

<style scoped>
/* ====================
   Todo listカード
==================== */
.todolist{
  width: 50%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border-radius: 10px;
  padding: 30px;
}
/* ====================
  タイトル・入力・リスト
==================== */
h1{
  font-size: 25px;
  margin-bottom: 20px;
}
.todoCreate{
  margin-bottom:20px;
}
.todo-input{
  width: 80%;
}
.todolists{
  margin-bottom: 5px;
}
.listItem{
  width: 30%;
}
/* ====================
        ボタン
==================== */
.button{
  background: #fff;
  font-weight: bold;
  padding: 0 16px;
  cursor: pointer;
}
.button:hover{
  color: #fff;
  transition: 0.3s;
}
.create-button{
  color: rgb(230, 115, 230);
  border: 2px solid rgb(230, 115, 230);
}
.create-button:hover{
  background-color: rgb(230, 115, 230);
}
.update-button{
  color:orange;
  border: 2px solid orange;
}
.update-button:hover{
  background-color: orange;
}
.delete-button{
  margin-left: 5px;
  color:rgb(131, 236, 214);
  border: 2px solid rgb(131, 236, 214);
}
.delete-button:hover{
  background-color: rgb(131, 236, 214);
}
</style>