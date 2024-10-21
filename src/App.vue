<template>
  <input v-model="input" />
  <button @click="addItem()">Add to list</button>
</template>

<script>
export default {
  data() {
    return {
      input: "",
      list: [],
    };
  },
  methods: {
    addItem() {
      this.list.push({
        id: this.list.length + 1,
        text: this.input,
        is_deleted: false,
      });
      this.input = "";
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
