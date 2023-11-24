<template>
  <div class="create-note">
    <h2>Create Note</h2>
    <form @submit.prevent="createNote">
      <div class="note-title">
        <label for="create-note-title">Title:</label>
        <input
          class="create-note-title"
          id="create-note-title"
          type="text"
          v-model="newNote.title"
          required />
      </div>
      <div class="note-todos">
        <legend for="create-todos-checkbox">Todo:</legend>
        <ul class="todos-list">
          <li
            class="todos-list-item"
            id="create-todos-checkbox"
            v-for="(todo, index) in newNote.todos"
            :key="index">
            <input
              class="create-todos-checkbox"
              type="checkbox"
              v-model="todo.completed" />
            <input class="create-todo-text" type="text" v-model="todo.text" />
          </li>
        </ul>
      </div>
      <div class="note-btns">
        <button class="add-todo-btn" type="button" @click="addTodo">
          Add Todo
        </button>
        <button class="save-note-btn" type="submit">Save Note</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newNote: {
        title: "",
        todos: [{ text: "", completed: false }],
      },
    };
  },
  methods: {
    addTodo() {
      this.newNote.todos.push({ text: "", completed: false });
    },
    createNote() {
      this.$emit("createNote", { ...this.newNote, id: Date.now() });
      this.newNote = {
        title: "",
        todos: [{ text: "", completed: false }],
      };
    },
  },
};
</script>

<style>
.create-note {
  padding: 15px;
}

.note-title {
  display: flex;
  flex-direction: column;
  padding: 10px;
  margin-left: 34px;
  margin-bottom: 10px;
}

.note-title label {
  margin-right: 35px;
}

.create-note-title,
.create-todo-text {
  width: 200px;
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  box-sizing: border-box;
}

.create-todo-text {
  margin-bottom: 5px;
}

.create-todo-text:focus,
.create-note-title:focus {
  border-color: black;
}

.note-btns {
  display: flex;
  justify-content: center;
}

.add-todo-btn,
.save-note-btn {
  justify-items: center;
  border: 1px solid;
  border-radius: 1.6rem;
  transition: box-shadow 0.2s;
  background-color: white;
  padding: 5px 10px;
  cursor: pointer;
}

.save-note-btn {
  border-color: darkgreen;
  color: darkgreen;
}

.add-todo-btn {
  margin-right: 5px;
}

.note-todos {
  margin-bottom: 15px;
}

.create-todos-checkbox {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  width: 15px;
  height: 8px;
  background-color: #fff;
  border: 1px solid #6611a7;
  border-radius: 4px;
  outline: none;
  cursor: pointer;
  margin-right: 15px;
}

.create-todos-checkbox:checked {
  background-color: #2b8a2e;
  border: 1px solid #2b8a2e;
}

.todos-list {
  margin: 0;
  padding: 0;
}

.todos-list-item {
  list-style: none;
}
</style>
