<template>
<div>
  
<input type="text" class="todo-input"
 placeholder="What Needs To be Done?" v-model="newTodo" @keyup.enter="addTodo">
 <div v-for="(todo, index) in todosFiltered":key= "todo.id" class="todo-item">
  <div class="todo-item-left">
    <input type="checkbox" v-model="todo.completed">
    <div class="todo-item-label" :class="{completed : todo.completed}" v-if="!todo.editing" @dblclick="editTodo(todo)">{{todo.title}} </div>   
    <input class="todo-item-edit" type="text" v-else v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus @keyup.esc="cancelEdit(todo)">  
  </div>
  
  <div class="remove-item" @click="removeTodo(index)">
   &times;
 </div>
 
 </div>
 <div class="extra-container">
<div>
<label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</label>

</div>
<div>
  {{remaining}} items left 
</div>
<div>
  <button :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
  <button :class="{active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
  <button :class="{active: filter == 'active'}" @click="filter = 'active'">Active</button>
</div>
<div>
  <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
</div>
 </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      filter: 'all',
      todos : [ {
        'id': 1,
        'title': 'Finish the lab',
        'completed': false,
        'editing': false,
      },
      {
        'id': 2,
        'title': 'Run the marathon',
        'completed': false,
        'editing': false,
      },
      ]
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed) 
      }
      else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed) 
      }
      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  directives: {
  focus: {
    inserted: function (el) {
      el.focus()
    }
  }
},
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return
      } 
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      })
      this.newTodo = '',
      this.idForTodo++
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
    editTodo(todo) {
      todo.editing = true
      this.beforeEditCache = todo.title
    },
    doneEdit(todo) {
      todo.editing = false
      if (todo.title.trim().length == 0) {
        todo.title = this.beforeEditCache
      } 
    },
    cancelEdit(todo) {
      todo.editing = false,
      todo.title = this.beforeEditCache
    },
    // clearAll(index) {
    //   this.todos.splice(0)
    // },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
   }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss'>
.logo {
  height: 70px;
  position: relative;
  left: 45%;
}
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline: 0;
  }
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color:rgb(255, 0, 0);
  }
}
.todo-item-left {
  display: flex;
  align-items: center;
}
.todo-items-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
}
.completed {
  text-decoration: line-through;
  color: grey;
}
.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;

&:hover {
  background: lightgreen;
}
&:focus {
  outline: none;
}
}
.active {
  background: lightgreen;
}
</style>
