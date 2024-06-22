<template>
  <section class="greeting">
    <div>
      <h2>welcome , <input type="text" class="name" placeholder="name" v-model="name">
      </h2>
    </div>
  </section>

  <h2 class="text">
    CREATE TO DO
  </h2>
  
  <section class="create">
      <p class="add-text">what do you want to add</p>
    <div>
      <input class="addTodo-input" type="text" placeholder="write here" v-model="input_content">
    </div>
    <button class="addToDo-button" @click="addTodo">submit</button>
  </section>

  <section class="list">
    <div class="todoDiv" v-for="todo in todos" :key="todo.content">
      <input class="check" type="checkbox" v-model="todo.done"> <input class="todoContent" type="text" v-model="todo.content">  <button @click="deleteTodo(todo)">Remove</button>
    </div>
  </section>

 
</template>



<script setup>
import {ref , watch , onMounted} from "vue"

const todos = ref([])
const name = ref ('')
const input_content = ref("") 

const addTodo = () => {
  if(input_content.value.trim() === '')
      return

  todos.value.push({
    content: input_content.value,
    done:false
  })
  input_content.value =""
} 

const deleteTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

watch(name , (newVal) => {
  localStorage.setItem("name" , newVal)
})
watch(todos , newVal => {
  localStorage.setItem("todos" , JSON.stringify(newVal))
}, {deep: true})

onMounted(() =>{
name.value = localStorage.getItem("name") || ""
todos.value = JSON.parse(localStorage.getItem("todos")) || []
})

</script>




<style>

:root{
  background-color: rgba(216, 216, 215, 0.37);
}

*{
  margin: 0;
  
  box-sizing: border-box;
	font-family: 'montserrat', sans-serif;
}

.greeting{
  margin: 10px;
  margin-bottom:15px ;
}
.name{
  border: none;
  background-color:rgba(216, 216, 215, 0); 
  font-size: 20px;
  font-weight:bold;
}
.text{
  margin-left: 10px;
  margin-bottom:5px ;
  font-weight: 100;
}
.create
{
  margin-left: 10px ;
}
.add-text{
  margin-bottom: 10px;
}
.addTodo-input
{
  border: none;
  width: 300px;
  height: 30px;
  padding-left:15px ;
  border-radius: 0.5rem;
}

.addTodo-input:focus
{
  outline: none;
}
.addToDo-button
{
  width: 300px;
  height: 20px;
  border-radius: 0.3rem;
  margin-bottom: 15px ;
}
.addToDo-button:hover
{
  background-color: white;
}
.todoDiv{
  margin-left:10px ;
  background-color: white;
  border:white solid 10px ;
  border-radius:0.3rem ;
  width: 500px;
  height: 40px;
  margin-bottom: 10px;
}



.todoDiv button{
  float: right;
  border: none;
  color: white;
  background-color: rgba(255, 0, 0, 0.774) ;
  padding:5px;
}
.todoDiv button:hover{
  background-color:rgba(255, 0, 0, 0.671) ;
}

.todoDiv input[type="checkbox"]
{
  margin-right: 10px;
}

.todoContent{
  border: none;
}
.todoDiv input[type="checkbox"]:checked + .todoContent{
  text-decoration: line-through;
}
</style>