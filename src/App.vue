<script setup>
  import {ref, onMounted, computed, watch} from "vue";

  const todos = ref([])
  const name = ref('')

  const input_content = ref('')
  const input_category = ref(null)

  const todos_asc = computed(() => todos.value.sort((a,b) => {
    return b.createdAt - a.createdAt
  }))

  const addTodo = () => {
    
    if (input_content.value.trim() === '' || input_category.value === null){
      return
    }


    todos.value.push({
      content: input_content.value,
      category: input_category.value,
      done: false,
      createdAt: new Date().getTime()
    })
    
  }

  const removeTodo = (todo) => {
    todos.value = todos.value.filter(t => t !== todo)
  }

  watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal));
  }, {deep: true})




  watch(name, (newVal) => {
    localStorage.setItem('name',newVal)
  })

  onMounted(() => {
    name.value = localStorage.getItem('name') || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })

</script>

<template>
  <main class="app">
    <section class="greeting">
      <h3 class="title">
        Hello, <input class="input-text name" type="text" placeholder="Name" v-model="name">
      </h3>
    </section>

    <section class="create-todo">
      
      <span class="label">Create a todo</span>
      <form @submit.prevent="addTodo">
        <h4 style="margin: 0; margin-bottom: 5px; margin-top: 10px; font-size: 12px;">What's on your todo list?</h4>
        <input class="input-text task" type="text" placeholder="e.g. make a video" v-model="input_content">
      
      
        <span class="label">Pick a category</span>
        <div class="options">
          
          <label for="">
            <div class="label-contents">
            <input 
              type="radio" 
              name="category"
              value="business" 
              v-model="input_category" />
              <span class="bubble business"></span>
              <div>Business</div>
          </div>
          </label>

          <label for="">
            <div class="label-contents">
              <input 
              type="radio" 
              name="category"
              value="personal" 
              v-model="input_category" />
              <span class="bubble personal"></span>
              <div>Personal</div>
            </div>
          </label>
        </div>

      <input class="input-submit" type="submit" value="Add todo">
      </form>
    </section>

    <section class="todo-list">
      <span class="">Todo List</span>
      <div v-for="todo in todos_asc" :class="`todo-item ${todo.done}`">
        <div class="todo-left-side">
          <label class="todo-checkbox">
          <input type="checkbox" v-model="todo.done"> <!-- links the checkbox to the corresponding todo.done value -->
          <span :class="`bubble ${todo.category}`"></span>
          </label>

          <div class="todo-content">
            <input class="input-text todos" type="text" v-model="todo.content">
          </div>
        </div>
        

        <div class="actions">
          <button class="delete" @click="removeTodo(todo)">Delete</button>
        </div>
      </div>

    </section>


    

  </main>
  
</template>
