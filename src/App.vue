<template>
  <div id="app">
    <NotesList />
  </div>
</template>

<script>
import NotesList from "@/components/NoteList.vue";

export default {
  components: {
    NotesList,
  },
  name: "App",
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
