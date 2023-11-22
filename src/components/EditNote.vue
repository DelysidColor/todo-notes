<template>
  <div class="edit-note">
    <h2>Edit Note</h2>
    <form @submit.prevent="updateNote">
      <div class="note-title">
        <label for="edit-note-title">Title:</label>
        <input
          class="edit-note-title"
          id="edit-note-title"
          type="text"
          v-model="editedNote.title"
          required />
      </div>
      <div class="note-todos">
        <label for="edit-todos-checkbox">Todo:</label>
        <ul class="todos-list">
          <li
            class="todos-list-item"
            v-for="(todo, i) in editedNote.todos"
            :key="i">
            <input
              class="edit-todos-checkbox"
              id="edit-todos-checkbox"
              type="checkbox"
              v-model="todo.completed" />
            <input class="edit-todo-text" type="text" v-model="todo.text" />
            <button
              class="del-todo-btn"
              @click.prevent="deleteTodo(todo)"></button>
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
  props: {
    initialNote: Object,
  },
  data() {
    return {
      editedNote: { ...this.initialNote },
    };
  },
  methods: {
    addTodo() {
      this.editedNote.todos.push({ text: "", completed: false });
    },
    updateNote() {
      this.$emit("updateNote", this.editedNote);
      this.moveCompletedToEnd();
      this.$emit("close");
    },
    moveCompletedToEnd() {
      this.editedNote.todos.sort((a, b) =>
        a.completed === b.completed ? 0 : a.completed ? 1 : -1
      );

      if (this.isNoteCompleted) {
        this.$emit("moveNoteToEnd", this.editedNote.id);
      }
    },
    deleteTodo(todo) {
      const index = this.editedNote.todos.findIndex((t) => t.id === todo.id);

      if (index !== -1) {
        this.editedNote.todos.splice(index, 1);
      }
    },
  },
  computed: {
    isNoteCompleted() {
      return this.editedNote.todos.every((todo) => todo.completed);
    },
  },
};
</script>

<style>
.edit-note {
  padding: 15px;
}

.note-title {
  display: flex;
  flex-direction: column;
  padding: 10px;
  margin-left: 24px;
  margin-bottom: 10px;
}

.note-title label {
  margin-right: 23px;
}

.edit-note-title,
.edit-todo-text {
  width: 200px;
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  box-sizing: border-box;
}

.edit-todo-text {
  margin-bottom: 5px;
}

.edit-todo-text:focus,
.edit-note-title:focus {
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

.edit-todos-checkbox,
.del-todo-btn {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  width: 15px;
  height: 8px;
  background-color: #fff;
  border: 1px solid #11a720;
  border-radius: 4px;
  outline: none;
  cursor: pointer;
  margin-right: 15px;
}

.del-todo-btn {
  margin-left: 10px;
  border-color: rgb(251, 41, 101);
}

.edit-todos-checkbox:checked {
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
