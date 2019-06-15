<template>
  <div id="app">
    <h1 class="mt-3">Oïganisation TOTODO</h1>
    <div class="container">
      <notifications group="i-am-not-part-of-any-group" position="left center" />

      <section>
         <div class="row justify-content-center mt-4">
           <input v-model="inputField" v-on:keyup.enter="addTodo" class="mr-1" placeholder="Totodo Item" />
           <button @mouseover="moveAround" v-bind:style="{position: addTodoBtnPosition, left: addTodoBtnLeft + 'px', bottom: addTodoBtnBottom + 'px'}" @click="addTodo" class="btn btn-primary">Add Totodo</button>
        </div>
      </section>

       <section class="container">
          <div class="row">
             <div class="offset-md-3 col-md-6 mt-3">
                <ul class="list-group justify-content-center">
                   <li class="row list-group-item border mt-2 col-xs-1" v-for="todo in todoList">
                      <div class="row align-items-center">
                        <input type="checkbox" v-on:change="toggle(todo)" v-bind:checked="todo.complete" class="col-sm-1 border border-danger">
                        <del v-if="todo.complete" class="col-sm-8">
                           <h5>{{ todo.name }}</h5>
                        </del>
                        <span v-else class="col-sm-8">
                           <h5>{{ todo.name }}</h5>
                        </span>
                        <span @click="deleteTodo(todo)" class="offset-sm-1 col-sm-2 delete text-right">X</span>
                      </div>
                   </li>
                </ul>
             </div>
          </div>
       </section>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import Notifications from 'vue-notification';

import BootstrapVue from 'bootstrap-vue';
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

import test from './components/test';

Vue.use(Notifications);

export default {
  name: 'app',
  components: {
     test
 },
  methods: {
    addTodo: function(todo) {
      todo = this.inputField;

      let fantasyTodo = this.applyFantasy(todo);

      if (fantasyTodo !== false) {
        this.todoList.push({name: fantasyTodo, complete: false});
      }
      this.inputField = '';
      console.log(this.todoList);
   },
   moveAround: function(event) {
      let now = new Date();

      if (now.getSeconds() > 5 && now.getSeconds() < 15) {
          Vue.notify({
            group: 'i-am-not-part-of-any-group',
            title: 'Seriously?',
            type: 'error',
            text: 'I am beaking my mxn'
          });

        this.addTodoBtnPosition = 'absolute';
        this.addTodoBtnBottom = (this.addTodoBtnBottom + 150) % 800;
        this.addTodoBtnLeft = 500;
      } else {
        this.addTodoBtnPosition = 'relative';
        this.addTodoBtnBottom = 0;
        this.addTodoBtnLeft = 0;
      }
   },
   applyFantasy: function(todo) {
      let random = Math.floor(Math.random() * 10),
          force = null,
          now = new Date();

      if (now.getSeconds() < 5 || now.getSeconds() > 55) random = 3;

      if (force !== null) random = force;

      // 1 Disparition
      if (random === 1) {
        return todo.replace('e', '');
      // 2 Take a beak
      } else if (random === 2) {
        return "Take a beak; you wok too much...";
      // 3 Im taking a beak
      } else if (random === 3) {
          Vue.notify({
            group: 'i-am-not-part-of-any-group',
            title: 'Listen dude',
            type: 'error',
            text: 'I take beak too!'
          });
          return false;
      } else {
        return todo;
      }
   },
   deleteTodo: function(todo) {
      var index = this.todoList.indexOf(todo);
      this.todoList.splice(index, 1);
      console.log(this.todoList);
   },
   toggle: function(todo) {
      todo.complete = !todo.complete;
   }
 },
  data () {
    return {
      inputField: '',
      addTodoBtnBottom: 100,
      addTodoBtnLeft: 100,
      addTodoBtnPosition: '',
      todoList: []
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

h5 {
   margin-bottom: 0px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.delete {
   color: pink;
   cursor: pointer;
}

.delete:hover {
   color: red;
}
</style>
