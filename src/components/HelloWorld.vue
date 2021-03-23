<template>
<div class="wrapper">
<h1>LKMX Front-End</h1>
  <div class="todo-container">
    <h2>To Do List</h2>
    <div class="todo-input-wrapper">
      <input type="text" class="todo-input" v-model="newTodo" @keyup.enter="addTodo">
      <button @click="addTodo" class="todo-input-button">Agregar</button>
    </div>
  <div class="list-wrapper">
    <div>
      <p :key="firstTodo" class="todo-item-label" :class="{'hidden' : !firstTodo}">Todavía no has agregado nada</p>
    </div>

    <div v-for="(todo, index) in todos" :key='todo.id' class="todo-item">
      <div class="todo-item-left">
          <div v-if="!todo.editable" class="todo-item-label"><li>{{ todo.title }}</li>
            <div class="remove-item">
              <img src="../assets/icon-trash.svg" @click="removeTodo(index)">
              <img src="../assets/icon-pen.svg" @click="editTodo(todo)">
            </div>
        </div>
      <div v-else class="todo-item-edit">
        <input type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.escape="cancelEdit(todo)" v-focus>
        <img src="../assets/icon-disk.svg" @click="editTodo(todo)">
      </div>
        </div>
    </div>
  </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data (){
    return {
      newTodo: '',
      idForTodo: null,
      beforeEditCache: '',
      firstTodo: true,
      todos: [
      ]
    }
    
  },
  directives:{
    focus:{
      inserted: function(el){
        el.focus()
      }
    }
  },
  methods: {
    addTodo() {
      if(this.newTodo.trim().length == 0){
        return
      }
      this.firstTodo = false
      this.todos.push(
        {
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
          editable: false,
        }),
        this.newTodo = ''
        this.idForTodo++
    },
    removeTodo(index){
      this.todos.splice(index, 1)
    },
    editTodo(todo){
      this.beforeEditCache = todo.title
      todo.editable = true
    },
    cancelEdit(todo){
      todo.title = this.beforeEditCache
      todo.editing = false
    },
    doneEdit(todo){
      if(todo.title.trim().length == 0){
        todo.title = this.beforeEditCache
      }
      todo.editable = false
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1{
  text-align: left;
  font-size: 48px;
  margin-bottom: 25px;
}
h2{
  text-align: left;
  font-size: 40px;
  margin-bottom: 12.5px;
}
p{
  font-size: 18px;
  font-weight: 500;
}
li{
  font-size: 18px;
  font-weight: 500;
}
.todo-container{
  padding: 8px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  background-color: #fff;
  box-shadow: 0px 2px 4px rgb(0 0 0 / 8%), 0px 0px 2px rgb(0 0 0 / 8%);
  border-radius: 8px;
  margin-left: 10px;
}
.wrapper{
  width: 500px;
  margin: 0 auto;
}
.todo-input-wrapper{
  display: flex;
  margin: 8px 0px;
}
.todo-input-button{
  padding: 9px;
  background-color: #424df7;
  color:white;
  border-radius: 8px;
  border: none;
  cursor: pointer;
}
.todo-input{
  padding: 8px;
  border-radius: 8px;
  width: 100%;
  margin-right:8px;
}
.todo-item-label{
  justify-content: space-between;
  display: flex;
  flex-direction: row;
  margin: 3px;
  padding: 4px;
  align-items: center;
}
img{
  padding: 4px;
  background-color:white;
  margin: 0px 2px;
  border: #706f6f 1px solid;
  border-radius: 8px;
}
.todo-item-edit{
  margin: 3px;
  padding: 4px;
  display: flex;
  justify-content: space-between;
}
.todo-item-edit input{
  width:100%;

}
.list-wrapper{
  background-color: #d3d3d3;
  padding: 4px;
  width: 98%;
  margin: 0 auto;
  border-radius: 8px;
}
.hidden{
  display: none;
}

/* .remove-item {
  background-image: url(../assets/icon-disk.svg);
} */
</style>
