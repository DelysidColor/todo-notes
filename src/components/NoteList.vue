<template>
  <div>
    <h2>Notes</h2>
    <div style="margin-bottom: 20px">
      <button class="add-note-btn" @click="openCreateModal">Add Note</button>
    </div>

    <div v-if="notes.length > 0">
      <NoteItem
        v-for="note in notes"
        :key="note.id"
        :note="note"
        @deleteNote="deleteNote"
        @editNote="openEditModal(note)" />
    </div>
    <div v-else>
      <p>
        It seems you haven't added any notes yet. <br />
        Please use the button "Add Note" above.
      </p>
    </div>

    <Modal :isOpen="isCreateModalOpen" @close="closeCreateModal">
      <CreateNote @createNote="createNote" />
    </Modal>

    <Modal :isOpen="isEditModalOpen" @close="closeEditModal">
      <EditNote
        :initialNote="selectedNote"
        @updateNote="updateNote"
        @moveNoteToEnd="moveNoteToEnd" />
    </Modal>
  </div>
</template>

<script>
import NoteItem from "@/components/NoteItem.vue";
import Modal from "@/components/ui/Modal.vue";
import CreateNote from "@/components/CreateNote.vue";
import EditNote from "@/components/EditNote.vue";

export default {
  components: {
    NoteItem,
    Modal,
    EditNote,
    CreateNote,
  },
  data() {
    return {
      isCreateModalOpen: false,
      isEditModalOpen: false,
      selectedNote: null,
      notes: [
        {
          id: 1,
          title: "Task One",
          todos: [
            { text: "Do this" },
            { text: "Do that" },
            { text: "And this one" },
            { text: "Also this" },
            { text: "And this in the end" },
          ],
        },
        {
          id: 2,
          title: "Task Two",
          todos: [{ text: "Do this" }],
        },
        {
          id: 3,
          title: "Task Three",
          todos: [{ text: "Do this" }, { text: "Do that" }],
        },
      ],
    };
  },
  methods: {
    openCreateModal() {
      this.isCreateModalOpen = true;
    },
    closeCreateModal() {
      this.isCreateModalOpen = false;
    },
    openEditModal(note) {
      this.selectedNote = { ...note };
      this.isEditModalOpen = true;
    },
    closeEditModal() {
      this.isEditModalOpen = false;
      this.selectedNote = null;
    },
    createNote(newNote) {
      this.notes.push(newNote);
      this.closeCreateModal();
    },
    deleteNote(noteId) {
      this.notes = this.notes.filter((note) => note.id !== noteId);
    },
    updateNote(updatedNote) {
      const index = this.notes.findIndex((note) => note.id === updatedNote.id);

      if (index !== -1) {
        this.notes.splice(index, 1, updatedNote);
      }

      this.closeEditModal();
    },
    moveNoteToEnd(noteId) {
      const noteIndex = this.notes.findIndex((note) => note.id === noteId);
      if (noteIndex !== -1) {
        const [movedNote] = this.notes.splice(noteIndex, 1);
        this.notes.push(movedNote);
      }
    },
  },
};
</script>

<style scoped>
.add-note-btn {
  justify-items: center;
  padding: 5px 10px;
  color: #6611a7;
  font-size: 16px;
  border: 2px solid #6611a7;
  border-radius: 20px;
  transition: box-shadow 0.2s;
  background-color: white;
}

.add-note-btn:hover {
  box-shadow: 0px 1px 20px rgba(226, 226, 226, 0.614);
}

h2 {
  color: #333;
  margin-bottom: 20px;
}
</style>
