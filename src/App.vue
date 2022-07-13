<template>
  <div class="container-form">
    <div class="container-title">
      <h1>My Todo App</h1>
    </div>
    <div class="control-section">
      <form class="input-section" @submit.prevent="addNewTodo">
        <input
          v-model="newTodo"
          name="newTodo"
          placeholder="Add a Todo"
          autocomplete="off"
        />
        <button class="todo-btn">Add</button>
      </form>
      <button class="allDone" @click="markAllDone">Mark All Done</button>
    </div>
    <ul class="todo-list">
      <li class="list-item" v-for="(todo, index) in todos" :key="todo.id">
        <div class="todo">
          <a
            class="markDone"
            :class="{ check: todo.done }"
            @click="toggleDone(todo)"
            ><i v-show="todo.done" class="fa-solid fa-check"></i
          ></a>
          <h3 :class="{ done: todo.done }">{{ todo.content }}</h3>
        </div>
        <div class="selectors">
          <Transition>
            <a
              v-show="todo.delete"
              @click="removeTodo(index)"
              class="deleteTodo"
              ><i class="fa-solid fa-trash-can"></i
            ></a>
          </Transition>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
import { ref } from "vue";

export default {
  setup() {
    const newTodo = ref("");
    const todos = ref([]);
    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value,
        delete: false,
      });
      newTodo.value = "";
    }
    function toggleDone(todo) {
      todo.done = !todo.done;
      setTimeout(() => {
        todo.delete = !todo.delete;
      }, 100);
    }
    function removeTodo(index) {
      todos.value.splice(index, 1);
    }
    function markAllDone() {
      todos.value.forEach((todo) => (todo.done = true));
    }

    return {
      todos,
      newTodo,
      addNewTodo,
      toggleDone,
      removeTodo,
      markAllDone,
    };
  },
};
</script>

<style>
body {
  font-family: sans-serif;
  padding-top: 1em;
  padding-bottom: 1em;
  font-size: 2em;
  width: 60%;
  margin: 0 auto;
  background-color: #fdebcc;
}
.container-form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
}
.container-title {
  display: flex;
  width: 100%;
  justify-content: center;
}
.control-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.input-section {
  display: flex;
  flex-direction: column;
  width: 100%;
  justify-content: center;
  align-items: center;
}
.input-section input {
  width: 100%;
  padding: 1rem 3rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1.5rem;
}
.todo-btn {
  padding: 0.5rem 4rem;
  margin: 0.5rem;
  border: none;
  border-radius: 2rem;
  background-color: #4d6ad1;
  color: #fffbf4;
  cursor: pointer;
  font-size: 1.5rem;
}

.todo-btn:hover {
  transition: 0.7s;
  background-color: #ccc;
  color: black;
}

.allDone {
  padding: 0.5rem 2rem;
  margin: 0.5rem;
  border: none;
  border-radius: 2rem;
  background-color: #d1774d;
  color: #fffbf4;
  cursor: pointer;
  font-size: 1rem;
}
.allDone:hover {
  transition: 0.7s;
  background-color: #ccc;
  color: black;
}

.todo-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  margin: 0;
  padding: 0;
}

.list-item {
  height: 3.5rem;
  width: 70%;
  border: none;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 1.5rem;
  padding: 0.5rem;
  background-color: white;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.todo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.selectors {
  display: flex;
  width: 3rem;
  height: 100%;
}

.markDone {
  width: 1.5rem;
  height: 1.5rem;
  border: solid 1px black;
  margin: 0;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: 0.4s;
}
.markDone:hover {
  background-color: #ccc;
}
.fa-solid {
  font-size: 1rem;
}
.deleteTodo {
  width: 100%;
  height: 100%;
  background-color: rgb(201, 107, 90);
  margin: 0;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.4s;
}
.deleteTodo:hover {
  background-color: rgb(209, 211, 89);
}
.done {
  text-decoration: line-through;
  color: grey;
}
.check {
  background: #62ce9d;
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

@media screen and (max-width: 500px) {
  .container-title h1 {
    font-size: 2rem;
  }
  .list-item {
    height: 2rem;
    width: 90%;
    font-size: 1rem;
  }

  .markDone {
    padding: 0.5rem;
  }
  .deleteTodo {
    padding: 0.5rem;
  }
}
</style>
