<template>
<div>

<button @click="changeTheme1()">Theme 1</button>
<button @click="changeTheme2()">Theme 2</button>
<button @click="changeTheme3()">Theme 3</button>

  <header>
      <section>
        <h2>Write your next task:</h2>
        <input type="text" v-model="newtask" @keyup.enter="pushTask()">
        <button @click="pushTask()">Send</button>
        <ul v-for="(task, index) of tasks" 
          :key="task.value">
          <li 
          @click="task.completed =! task.completed"
          :style="[task.completed ? {'text-decoration': 'line-through'} : {'text-decoration': 'none'}]">
            {{task.name}}
          </li>
          <button @click="removeTask(index)">D</button>
        </ul>
      </section>
  </header>

</div>
</template>

<script>
export default({
  el: '#app',
  data(){
    return{

    tasks: [
      {name: 'Exercise', completed: false},
      {name: 'Learn Vue.js', completed: true}
    ],
    newtask: '',
    picked: null,
    }
  },

  methods: {
    pushTask(){
      if(this.newtask !== '') {
      this.tasks.push({
        name: this.newtask, completed: false
      })
      this.newtask = ''
      }
    },
    removeTask: function(index) {
      this.tasks.splice(index, 1);
    },
    changeTheme1(){
      document.documentElement.setAttribute("data-theme", "first")
      localStorage.setItem("data-theme", "first")},
    changeTheme2(){
      document.documentElement.setAttribute("data-theme", "second")
      localStorage.setItem("data-theme", "second")},
    changeTheme3(){
      document.documentElement.setAttribute("data-theme", "third")
      localStorage.setItem("data-theme", "third")
    }
  },

  mounted(){
    let localTheme = localStorage.getItem("data-theme")
    document.documentElement.setAttribute("data-theme", localTheme)
  }
})
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;700&display=swap');

html{
  background-color: var(--background);
}
#app {
  font-family: 'inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--color-primary);
}

//color scheme
[data-theme="first"]{
    --color-primary: #FFD500;
    --color-accent: #2CD997;
    --background: #AF7DFF; 
}
[data-theme="second"]{
    --color-primary: #ffe5d4;
    --color-accent: #8b6278;
    --background: #efc7c2;
}
[data-theme="third"]{
    --color-primary: #abd1c6;
    --color-accent: #e16162;
    --background: #004643;
}
</style>