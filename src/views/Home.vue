<template>
  <div class="home">
      <section>
        <h2>Write your next task:</h2>
        <input type="text" v-model="newtask" @keyup.enter="pushTask()">
        <button @click="pushTask()">Send</button>
        <ul v-for="(task, index) of tasks" 
          :key="task.value">
          <li 
          @click="task.completed =! task.completed"
          :style="[task.completed ? {'text-decoration': 'line-through'} : {'text-decoration': 'none'}]">
            {{task.id}}, {{task.name}}
          </li>
          <button @click="removeTask(index)">D</button>
        </ul>
      </section>
  </div>
</template>

<script>

export default {
  name: 'Home',
}
</script>
