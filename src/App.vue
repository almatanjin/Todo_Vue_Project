<script setup>

import { ref, computed,onMounted ,watch } from 'vue';

const todos = ref([])
const name = ref("")


const input_content = ref("")
const input_category = ref(null)


function removeTodo(todo)
{
  todos.value = todos.value.filter(checkTodo)

  function checkTodo(t){
    return t !== todo
  }
}



const todos_asc = computed(()=> todos.value.sort((a,b)=>{
  return a.createdAt - b.createdAt
}))

const addTodo = () => {

  if(input_content.value.trim() === " " || input_category.value === null){
    return alert("Please select your Todo list and a category")
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done:false,
    createdAt: new Date().getTime()
  })
  // console.log('todos',JSON.stringify(newvalue))
}

watch(todos , (newvalue) =>{
  localStorage.setItem('todos', JSON.stringify(newvalue))
}, { deep: true}), 

watch(name , (newvalue) =>{
  localStorage.setItem('name',newvalue)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})


</script>

<template>
 <main class="app">
  <section class="greeting">
    <h2 class="title">
      Whats up,
      <input type="text" placeholder="Name here" id="whatsup" v-model="name" />

    </h2>
  </section>
  <section class="create-todo">
    <h3>CREATE A TODO</h3>

    <form @submit.prevent="addTodo">
      <h4>Whats on your todo list</h4>
      <input type="text" v-model="input_content" placeholder="e.g make a video" />

    <h4>Pick a category</h4>

    <div class="options">

      <label>
        <input type="radio" name="category"  value="business" v-model="input_category"/>
        <span class="bubble business"></span>
        <div>Business</div>
      </label>
      <label>
        <input type="radio" name="category"  value="personal" v-model="input_category"/>
        <span class="bubble personal"></span>
        <div>Personal</div>
        
      </label>
      

    </div>

    <input type="submit" value="Add todo" />

      
    </form>
  </section>
  
 </main>
 <section class="todo-list">
  <h3>Todo ist</h3>
  <div class="list">
    <div v-for="todo in todos_asc" :key="todo" :class="`todo-item ${todo.done && 'done'}`">
      <label>
        
        <div class="todo-content">
          <input type="text" v-model="todo.content" />
        </div>
        <div class="actions">
          <button class="delete" @click="removeTodo(todo)">Delete</button>
        </div>
      </label>



    </div>
  </div>
 </section>
</template>

<style scoped>

</style>
