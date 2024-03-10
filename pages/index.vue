<template>
  <div id="app">
    <div class="header">
      <h1>AirTribe Internship Assignment</h1>
    </div>
    <div class="container">
      <div class="column" @drop="drop($event, 'list')" @dragover.prevent>
        <div class="section">
          <h2 style="background-color: lightgreen">List ({{ listTodos.length }})</h2>
          <div class="card-container">
            <div class="card" v-for="(todo, index) in listTodos" :key="'list' + index" draggable="true" @dragstart="dragStart($event, 'list', index)">
              <div class="card-content">
                <h3>{{ todo.text }}</h3>
              </div>
            </div>
          </div>
          <input type="text" v-model="newTodoList" placeholder="New" @keyup.enter="addTodo('list', newTodoList)" />
        </div>
      </div>
      <div class="column" @drop="drop($event, 'inProgress')" @dragover.prevent>
        <div class="section">
          <h2 style="background-color: lightyellow">In-Progress ({{ inProgressTodos.length }})</h2>
          <div class="card-container">
            <div class="card" v-for="(todo, index) in inProgressTodos" :key="'inProgress' + index" draggable="true" @dragstart="dragStart($event, 'inProgress', index)">
              <div class="card-content">
                <h3>{{ todo.text }}</h3>
              </div>
            </div>
          </div>
          <input type="text" v-model="newTodoInProgress" placeholder="New" @keyup.enter="addTodo('inProgress', newTodoInProgress)" />
        </div>
      </div>
      <div class="column" @drop="drop($event, 'completed')" @dragover.prevent>
        <div class="section">
          <h2 style="background-color: lightcoral">Completed ({{ completedTodos.length }})</h2>
          <div class="card-container">
            <div class="card completed" v-for="(todo, index) in completedTodos" :key="'completed' + index" draggable="true" @dragstart="dragStart($event, 'completed', index)">
              <div class="card-content">
                <h3>{{ todo.text }}</h3>
              </div>
            </div>
          </div>
          <input type="text" v-model="newTodoCompleted" placeholder="New" @keyup.enter="addTodo('completed', newTodoCompleted)" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodoList: '',
      newTodoInProgress: '',
      newTodoCompleted: '',
      listTodos: [],
      completedTodos: [],
      inProgressTodos: [],
      draggedItem: null,
      draggedFrom: null,
    };
  },
  methods: {
    addTodo(section, text) {
      if (text.trim() !== '') {
        if (section === 'list') {
          this.listTodos.push({ text: text, completed: false });
          this.newTodoList = '';
        } else if (section === 'inProgress') {
          this.inProgressTodos.push({ text: text, completed: false });
          this.newTodoInProgress = '';
        } else if (section === 'completed') {
          this.completedTodos.push({ text: text, completed: true });
          this.newTodoCompleted = '';
        }
      }
    },
    deleteTask(index, category) {
      if (category === 'list') {
        this.listTodos.splice(index, 1);
      } else if (category === 'inProgress') {
        this.inProgressTodos.splice(index, 1);
      } else if (category === 'completed') {
        this.completedTodos.splice(index, 1);
      }
    },
    dragStart(event, category, index) {
      this.draggedItem = index;
      this.draggedFrom = category;
    },
    drop(event, category) {
      if (this.draggedFrom !== category) {
        let todo;
        if (this.draggedFrom === 'list') {
          todo = this.listTodos.splice(this.draggedItem, 1)[0];
        } else if (this.draggedFrom === 'inProgress') {
          todo = this.inProgressTodos.splice(this.draggedItem, 1)[0];
        } else if (this.draggedFrom === 'completed') {
          todo = this.completedTodos.splice(this.draggedItem, 1)[0];
        }

        if (category === 'list') {
          this.listTodos.push(todo);
        } else if (category === 'inProgress') {
          this.inProgressTodos.push(todo);
        } else if (category === 'completed') {
          this.completedTodos.push(todo);
        }
      }
    },
  },
};
</script>

<style scoped>
/* App styles */
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
}

.header {
  background-color: #333;
  color: #fff;
  padding: 20px;
}

.container {
  display: flex;
  justify-content: space-between;
  margin: 20px auto;
  max-width: 1200px;
}

.column {
  flex: 1;
  margin: 0 10px;
}

.section {
  display: flex;
  flex-direction: column;
}

.card-container {
  display: flex;
  flex-direction: column;
}

.card {
  width: calc(100% - 20px);
  background-color: #fff;
  margin: 10px 0;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
  cursor: pointer;
}

.card.completed h3 {
  text-decoration: line-through;
}

.card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.card-content {
  padding: 10px;
}

input[type="text"] {
  width: calc(100% - 20px);
  margin: 10px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

input[type="text"]:focus {
  outline: none;
  border-color: #333;
}

button {
  background-color: #f44336;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  margin-top: 5px;
}

button:hover {
  background-color: #d32f2f;
}
</style>
