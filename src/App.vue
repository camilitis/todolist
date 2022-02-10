<template>
<div>

  <section id="bar">
    <div class="colorscheme-bar">
        <input type="radio" id="radio-one" name="switch-one" value="colorscheme1" ref="theme1" @change="changeTheme()"/>
        <label id="colorscheme1" class="colorscheme colorThemeSquare button" for="radio-one"></label>
        <input type="radio" id="radio-two" name="switch-one" value="colorscheme2" ref="theme2" @change="changeTheme()"/>
        <label id="colorscheme2" class="colorscheme colorThemeSquare button" for="radio-two"></label>
        <input type="radio" id="radio-three" name="switch-one" value="colorscheme3" ref="theme3" @change="changeTheme()"/>
        <label id="colorscheme3" class="colorscheme colorThemeSquare button" for="radio-three" ></label>
    </div>
      <p class="button" @click="$refs.modalName.openModal()">Instructions</p>
  </section>
  <header>
    <div id="title" class="window">
      <h1>To do list</h1>
    </div>
      <section class="window write">
        <h2>Write your next task:</h2>
          <div id="inputSection">
            <input type="text" v-model="newtask" @keyup.enter="pushTask()">
            <div id="inputSectionButtons">
              <button class="button taskButton" @click="clearInput()" role="button"  ontouchstart="">Clear</button>
              <button class="button taskButton" @click="pushTask()" role="button"  ontouchstart="">Send</button>
            </div>
          </div>
      </section>
  </header>
    <section id="taskboard">
      <span 
        v-for="(task, index) of tasks" 
        :key="task.value"
        class="task"
      >
        <div 
          class="text"
          @click="task.completed =! task.completed; saveCompleted()"
          :style="[task.completed ? {'text-decoration': 'line-through', 'text-decoration-thickness': '2.5px'} : {'text-decoration': 'none'}]"
        >
          {{task.name}} 
        </div>
        <button @click="removeTask(index)" class="button">Delete</button>
      </span>
    </section>
</div>

<p id="snackbar" class="window4">Please write your new task</p>

<modal ref="modalName">
    <template v-slot:header>
      <h3>Instructions</h3>
    </template>

    <template v-slot:body>
      <p>Write your task and press enter (or send). When task is finished you can click on it to mark it as "done" and then delete it</p>

      <footer>Made by <a href="camilaguerra.vercel.app" target="_blank">Camila</a> with Vue.js</footer>
    </template>

</modal>

</template>

<script>
import Modal from "@/components/Modal"

export default({
  el: '#app',
  components: {
    Modal 
  },
  data(){
    return{
    tasks: [
      {id: 1643822542708, name: 'Exercise', completed: false},
      {id: 1643822582580, name: 'Learn Vue.js', completed: true}
    ],
    newtask: '',
    picked: null
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
    changeTheme(){
      if(this.$refs.theme1.checked == true) {
        document.documentElement.setAttribute("data-theme", "first")
        localStorage.setItem("data-theme", "first")
      }else if(this.$refs.theme2.checked == true){
        document.documentElement.setAttribute("data-theme", "second")
        localStorage.setItem("data-theme", "second")
      }else{
        document.documentElement.setAttribute("data-theme", "third")
        localStorage.setItem("data-theme", "third")
      }
    },

    zoom() {
      document.body.style.zoom = "90%" 
    }
  },

  created(){
    let data = JSON.parse(localStorage.getItem("localTasks"))
    if(data != null){
      this.tasks = data
    }

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
  padding: .3rem;
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
  transition: all .3s ease;
  a{
    text-decoration: none;
    border-bottom: 2px dotted var(--color-accent);
    transition: all .3s ease;
    &:hover{
      border-bottom: 2px dotted black;
    }
  }
}

//Color Theme
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