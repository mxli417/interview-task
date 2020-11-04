<template>
  <div id="app-wrapper">
    <div class="first-container">
      <h1>TO-DO List</h1>
      <p style="margin-bottom:0.5cm"><input class="input" placeholder="Write your task here" maxlength="50" v-model="newToDo"></p>
      <p style="margin-bottom:0.5cm"><button class="add-button" @click="addToDo()">Add task</button></p>
    </div>
    <div class="second-container">
      <h2>Not finished tasks</h2>
      <ul class="dual-list-current">
        <li v-for="(todo, i) in existingToDo" v-bind:key="todo.id">
          <div class="title"><input type="checkbox" id="todo.id" @click=accomplishToDo(i)> {{todo.text}}</div>
          <div class="content"><button class="delete-button" @click="wipeToDo(i)"><b>&#10006;</b></button></div>
        </li>
      </ul>
      <p style="margin-bottom:0.6cm"></p>
      <h2>Finished tasks</h2>
      <ul class="dual-list-finished">
        <li v-for="(todo, i) in finishedToDo" v-bind:key="todo.id"> 
          <div class="title"><input type="checkbox" id="todo.id" checked disabled> {{todo.text}} </div>
          <div class="content"><button class="delete-button" @click="finishToDo(i)"><b>&#10006;</b></button></div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  // initial batch of Todos data for both lists
  data () {
    return {
      newToDo: null, 
      existingToDo:[
        {text: 'Call Adam', id:0},
        {text: 'Buy flowers', id:1}, 
        {text: 'Send email too John', id:2}
      ],
      finishedToDo:[
        {text: 'Buy concert tickets', id:0},
        {text: 'Call dad', id:1}
      ]
    }
  },
  // check if there is already a local storage and fetch list items from there
  mounted() {
    if (localStorage.getItem('existingToDo')) {
      try {
        this.existingToDo = JSON.parse(localStorage.getItem('existingToDo'));
      } catch(e) {
        localStorage.removeItem('existingToDo');
      }
    }
    if (localStorage.getItem('finishedToDo')) {
      try {
        this.finishedToDo = JSON.parse(localStorage.getItem('finishedToDo'));
      } catch(e) {
        localStorage.removeItem('finishedToDo');
      }
    }
  },
  methods:{
    // adds Todos to the existing ToDos List and saves both lists
    addToDo(){
      if (!this.newToDo) {
        return;
      }
      this.existingToDo.push({
        text: this.newToDo, 
        id: new Date().valueOf()
      }),
      this.newToDo=''
      this.saveToDos()
    },
    // pushes Todos to the finished ToDos List after achievement and saves both lists
    accomplishToDo(i){
      this.finishedToDo.push({
        text: this.existingToDo[i].text,
        id: new Date().valueOf()
      })
      this.existingToDo.splice(i, 1)
      this.saveToDos()
      this.savefToDos()
    },
    // wipes Todos from the existing Todos list if user doesnt need them
    wipeToDo(i) {
      this.existingToDo.splice(i, 1)
      this.saveToDos()
      this.savefToDos()
    }, 
    // wipes Todos from the finished Todos list if user doesnt need them anymore
    finishToDo(i){
      this.finishedToDo.splice(i, 1)
      this.savefToDos()
    },
    // saves Todos locally
    saveToDos() {
      const parsed = JSON.stringify(this.existingToDo)
      localStorage.setItem('existingToDo', parsed)
    },
    // loads Todos if saved locally
    savefToDos() {
      const parsed = JSON.stringify(this.finishedToDo)
      localStorage.setItem('finishedToDo', parsed)
    }
  }
}
</script>

<style>
#app-wrapper {
  margin-right: auto;
  margin-left: auto;
  max-width: 250px;
  background-color: white;
}

.first-container {
  display: block;
  justify-content: center;
  text-align: center;
  margin-right: auto;
  margin-left: auto;
  background: white;
  margin: 0 auto;
}

.second-container {
  display: block;
  justify-content: center;
  text-align: left;
  margin-right: auto;
  margin-left: auto;
  background: white;
  margin: 0 auto;
}

ul.dual-list-current {
  margin: 0; 
  padding: 0;
  font-family: 'Roboto', sans-serif;
  font-weight: lighter;
}

ul.dual-list-current > li { 
  display: block; 
  height: 30px; 
  line-height: 30px;
  max-width: 250px;
  border: 0px solid black;
  margin: 0 0 10px 0;
  -moz-box-shadow: 2px 3px 2px grey;
  -webkit-box-shadow: 2px 3px 2px grey;
  box-shadow: 2px 3px 2px grey;
}
ul.dual-list-current > li > div.title { float: left; height: 25px; vertical-align: baseline; }
ul.dual-list-current > li > div.content { float: right; height: 25px; vertical-align: baseline; }

ul.dual-list-finished {
  margin: 0; 
  padding: 0;
  color: lightgray;
  font-family: 'Roboto', sans-serif;
  font-weight: lighter;
}

ul.dual-list-finished > li { 
  display: block; 
  height: 30px; 
  line-height: 30px;
  max-width: 250px;
  border: 0px solid black;
  margin: 0 0 10px 0;
  -moz-box-shadow: 2px 3px 2px grey;
  -webkit-box-shadow: 2px 3px 2px grey;
  box-shadow: 2px 3px 2px grey;
}
ul.dual-list-finished > li > div.title { float: left; height: 25px; vertical-align: baseline; text-decoration: line-through;}
ul.dual-list-finished > li > div.content { float: right; height: 25px; vertical-align: baseline; }

h1{
  color:gray;
  font-size: 18px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: lighter;
}

h2{
  color:gray;
  font-size: 14px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: lighter;
}

.input {
  border-radius: 5px;
  border: none;
  width: 200px;
  height: 30px;
  color: lightgray;
  font-size: 14px;
  -moz-box-shadow: 2px 3px 2px grey;
  -webkit-box-shadow: 2px 3px 2px grey;
  box-shadow: 2px 3px 2px grey;
  font-family: 'Roboto', sans-serif;
  text-indent: 10px;
}

.add-button {
  border-radius: 25px;
  width: 85px;
  height: 30px;
  background: #32cd32;
  border: none;
  color: white;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  font-family: 'Roboto', sans-serif;
}

.delete-button {
  border: None;
  background-color: white;
  font-family: 'Roboto', sans-serif;
}
</style>
