<template>
<div>

  <section id="bar">
    <div class="colorscheme-bar">
      <div id="colorscheme1" class="colorscheme bar-element" @click="changeTheme1()" ontouchstart=""></div>
      <div id="colorscheme2" class="colorscheme bar-element" @click="changeTheme2()" ontouchstart=""></div>
      <div id="colorscheme3" class="colorscheme bar-element" @click="changeTheme3()" ontouchstart=""></div>
    </div>
    <div id="instructions">
      <p class="bar-element">Instructions</p>
    </div>
  </section>

  <header>
    <div id="title"><h1>To do list</h1></div>
    <section class="window write">
      <h2>Write your next task:</h2>
        <div class="inputbutton">
          <input type="text" v-model="newtask" @keyup.enter="pushTask()">
          <button class="button" @click="pushTask()" role="button"  ontouchstart="">Send</button>
          <button class="button" @click="clearInput()" role="button"  ontouchstart="">Clear</button>
        </div>
    </section>
  </header>

    <ul id="taskboard" v-for="(task, index) of tasks" :key="task.value">
      <span @click="task.completed =! task.completed; saveCompleted()" class="task window-less w-animation">
        <li :style="[task.completed ? {'text-decoration': 'line-through', 'text-decoration-thickness': '2px'} : {'text-decoration': 'none'}]">
          {{task.name}} 
        </li>
        <button class="button" @click="removeTask(index)">Delete</button>
      </span>
    </ul>
</div>

<p id="snackbar">Please write your new task</p>

</template>

<script>
export default({
  el: '#app',
  data(){
    return{
    tasks: [
      {id: 1643822542708, name: 'Exercise', completed: false},
      {id: 1643822582580, name: 'Learn Vue.js', completed: true}
    ],
    newtask: '',
    picked: null,
    }
  },

  methods: {
    pushTask(){
      if(this.newtask == ''){
          var x = document.getElementById("snackbar")
          x.className = "show"
          setTimeout(function(){
              x.className = x.className.replace("show", "")}, 3000)
        }else{
          this.tasks.push({
            id: String(Date.now()), name: this.newtask, completed: false
          })
        localStorage.setItem("localTasks", JSON.stringify(this.tasks))
        this.newtask = ''
        }
      },
    clearInput(){
      this.newtask = ''
    },
    removeTask: function(index) {
      this.tasks.splice(index, 1)
      localStorage.removeItem("localTasks", this.tasks)
    },
    saveCompleted(){
      localStorage.setItem("localTasks", JSON.stringify(this.tasks))
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

  created(){
    let data = JSON.parse(localStorage.getItem("localTasks"))
    if(data != null){
      this.tasks = data
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
@import "global.scss";

input, textarea, button, select, a, text {
  -webkit-tap-highlight-color: var(--background);
  color: black;
}

html{
  background-color: var(--background);
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
     -moz-user-select: none; /* Firefox */
      -ms-user-select: none; /* Internet Explorer/Edge */
          user-select: none;
}
#app {
  font-family: 'inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding: .5rem;
}

//color scheme
:root {
    --color-primary: #FFD500;
    --color-accent: #2CD997;
    --background: #AF7DFF;
}
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