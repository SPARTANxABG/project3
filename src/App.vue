<script setup>
  import {ref,onMounted, watch} from 'vue'

  const myArray = ref([])
  const name = ref('')
  const input_number = ref('')
  const input_name = ref('')

  const addContact = () =>{
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

  const removeContact = (x) => {
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
        Welcome back: <input type="text" placeholder="Enter Name" v-model="name">
      </h2>  
    </section>

    <section class="create-contact">
      <h3>CONTACT LIST</h3>
      <form @submit.prevent = "addContact">
        <h4>Add Phone Number Here:</h4>
        <input type="text" placeholder="(xxx) xxx-xxxx" v-model="input_number"/>

        <h4>Add Name Here:</h4>
        <input type="text" placeholder="Bob Sponge" v-model="input_name"/>
        <input type="submit" value="Create Contact"/>
      </form>

    </section>

    <section class="contact-list">
      <div class="list">
        <div v-for="x in myArray" :class="`todo-item ${x.done ? 'done' : 'not-done'}`" :key="x">

          <div class="todo-content">
            <input type="text" v-model="x.content"/>
            <input type="text" v-model="x.category"/>
          </div>

          <div class="actions">
            <button class="delete"@click="removeContact(x)">Delete</button>
          </div>

        </div>

      </div>

    </section>

  </main>

</template>
