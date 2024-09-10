<script setup>
  import {ref,onMounted, watch} from 'vue'

  const myArray = ref([])
  const name = ref('')
  const number = ref('')
  const input_name = ref(null)

  const addTodo = () =>{
    if(input_number.value.trim() === '' || input_name.value === ''){
      return
    }
    
    myArray.value.push({
      content: input_number.value,
      category: input_name.value,
      done: false,
    })

    input_number.value = ''
    input_name.value = ''
  }

  const removeTodo = (x) => {
    myArray.value = myArray.value.filter(Element => Element !== x)
  }

  onMounted( () =>{
    name.value = localStorage.getItem('name') || ''
    myArray.value = JSON.parse(localStorage.getItem('myArray')) || []
  }
  )

  watch(name, (newVal) => {
    localStorage.setItem('name', newVal)
  })

  watch(myArray, (newVal) => {
    localStorage.setItem('myArray', JSON.stringify(newVal))
  }, {deep: true})

</script>

<template>

  <main class="app">

    <section class="greeting">
      <h2 class="title">
        Welcome back, <input type="text" placeholder="Enter Name" v-model="name">
      </h2>  
    </section>

    <section class="create-todo">
      <h3>CONTACT LIST</h3>
      <form @submit.prevent = "addTodo">
        <h4>Add Phone Number Here:</h4>
        <input type="text" placeholder="(xxx) xxx-xxxx" v-model="input_number"/>

        <h4>Add Name Here:</h4>
        <input type="text" placeholder="Bob Sponge" v-model="input_name"/>
      </form>

    </section>

    <section class="todo-list">
      <div class="list">
        <div v-for="x in myArray" :class="`todo-item ${x.done ? 'done' : 'not-done'}`" :key="x">

          <label>
            <input type="checkbox" v-model="x.done"/>
            <span :class="`bubble ${x.category}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="x.content"/>
          </div>

          <div class="actions">
            <button class="delete"@click="removeTodo(x)">Delete</button>
          </div>

        </div>


      </div>



    </section>


  </main>

</template>
