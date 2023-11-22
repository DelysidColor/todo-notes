<template>
  <div class="note">
    <div class="single-note">
      <h3 :class="{ completed: isNoteCompleted }">{{ note.title }}</h3>
      <ul>
        <li
          class="todo-text"
          v-for="(todo, index) in note.todos.slice(0, 3)"
          :key="index"
          :class="{ completed: todo.completed }">
          {{ todo.text }}
        </li>
      </ul>
    </div>
    <div class="note-btns">
      <button class="edit-button" @click="editNote">edit</button>
      <button class="delete-button" @click="confirmDelete">del</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    note: Object,
  },
  computed: {
    isNoteCompleted() {
      return this.note.todos.every((todo) => todo.completed);
    },
  },
  methods: {
    editNote() {
      this.$emit("editNote", this.note.id);
    },
    confirmDelete() {
      if (window.confirm("You want to delete this Note?")) {
        this.$emit("deleteNote", this.note.id);
      }
    },
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}

.note {
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 300px;
  margin: 0 auto;
  border-bottom: 1px solid #ddd;
  padding: 15px;
}

.note h3 {
  color: #555;
  margin: 0;
}

.note ul {
  list-style-type: none;
  padding: 0;
}

.todo-text {
  margin-bottom: 5px;
  font-size: 14px;
  line-height: 12px;
  margin-bottom: 10px;
  text-align: start;
}

.single-note {
  margin-right: 20px;
}

.note-btns {
  display: flex;
}

.edit-button,
.delete-button {
  justify-items: center;
  border: 1px solid;
  border-radius: 1.6rem;
  transition: box-shadow 0.2s;
  background-color: white;
  padding: 10px 5px;
  cursor: pointer;
  writing-mode: vertical-lr;
  text-orientation: upright;
  white-space: nowrap;
}

.edit-button:hover,
.delete-button:hover {
  box-shadow: 0px 1px 20px rgba(226, 226, 226, 0.614);
}

.edit-button {
  border-color: #6611a7;
  color: #6611a7;
  margin-right: 10px;
}

.delete-button {
  border-color: black;
}
</style>
