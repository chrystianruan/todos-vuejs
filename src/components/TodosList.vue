<template>
<div class="container">
    <div class="div-title">
        <h1>TODOs</h1>
    </div>

    <div class="box">
      <form v-on:submit.prevent="newTodo()">
        <input type="text" class="input-todo" v-model="name">
        <button type="button" class="btn-send" v-on:click="newTodo()">Enviar</button>
      </form>
    </div>

    <div class="list">

      <ul>
        <li v-for="todo in todos" :key="todo.id">
          <div class="box-list"> 

            <div class="tasks">
              <input v-if="todo.realized == 0" type="checkbox" v-on:click="updateSituationTodo(todo.id)">
              <input v-else checked type="checkbox" v-on:click="updateSituationTodo(todo.id)">

              <span v-if="todo.realized == 0" v-on:click="updateSituationTodo(todo.id)"> {{ todo.name }} </span>
              <span v-else class="span-realized" v-on:click="updateSituationTodo(todo.id)"> {{ todo.name }} </span>
             
              
            </div>

            <div class="icons"> 
              <i class="fa-solid fa-pen-to-square" v-on:click="editTodo(todo.id)"></i> 
              <i class="fa-solid fa-trash" v-on:click="removeTodo(todo.id)"></i>
            </div>

          </div>
          
        </li>  
      </ul>

    </div>

</div>

<div class="modal" id="modal">
  <div class="title-modal"><h4>Edição de Todo</h4></div>
  <hr>
  <div class="body-modal">
    <form v-on:submit.prevent="updateTodo()">
        <input type="hidden" v-model="editId">
        <input type="text" class="input-todo" v-model="editName">
        <button type="button" class="btn-send" v-on:click="updateTodo()">Alterar</button>
    </form>
  </div>
</div>

</template>


<script>
export default {
    name: 'TodosList',
    data() {
        return {
            todos: [],
            name: '',
            editName: '',
            editId: '',
        }
    }, 
    methods: {
        newTodo() {
          if (this.name.trim() === '') {
            return;
          }
            this.todos.push({
                id: Math.random().toString(36).slice(2),
                name: this.name,
                realized: 0
            });

            this.name = ''
        },

        updateSituationTodo(id) {
          let selected = this.todos.find(todo => todo.id === id);
          
          if (selected.realized == 0) {
            selected.realized = 1;
          } else {
            selected.realized = 0;
          }
  
        }, 

        editTodo(id) {
          let modal = document.getElementById('modal');
          modal.style.display = "block";
          let selected = this.todos.find(todo => todo.id === id);

          this.editName = selected.name;
          this.editId = id;

        },

        updateTodo() {
          if (this.editName.trim() === '') {
            return;
          }
          let selected = this.todos.find(todo => todo.id === this.editId);

          selected.name = this.editName;

          let modal = document.getElementById('modal');
          
          this.editName = '';
          this.editId = '';
          modal.style.display = "none";  
        },

        removeTodo(id) {

          let index = this.todos.findIndex(todo => todo.id === id);

          this.todos.splice(index, 1);

        } 
    }, 
    computed: {
     
        
    }

}
</script>


<style>

.input-todo {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 10px 0px 0px 10px;
}
.btn-send {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 0px 10px 10px 0px;
  cursor: pointer;
}
.btn-send:hover {
  background-color: #ac8282;
}
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 100%;

}
.modal {
    display: none;
    background-color: #cccccc;
    border: 1px solid #000;
    border-radius: 10px;
    padding: 25px;
    width: 80%;
    position: absolute;
    top: 20%;
    left: 50%;
    transform: translate(-50%, -50%);
    
}


.list {
  margin: 10px auto;
}

li {
  list-style-type: none; 
}

.box-list {
  display: flex;
  grid-gap: 10px;
}

.icons {
  display: flex;
  grid-gap: 10px;
  margin: 0 5px;
}

i {
  cursor: pointer;
  font-size: 1.4em;
}

.tasks {
  display: flex;
  grid-gap: 5px;
  font-size: 1.4em;
}

.span-realized {
  text-decoration: line-through;
  color: #ccc;
}

@media (min-width: 768px) {
    .modal {
        width: 40%;
    }
}

</style>