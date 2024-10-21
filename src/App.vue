<template>
  <h1>Nova polozka</h1>

  <p style="margin-bottom: 0; font-size: 90%">Nazov polozky</p>
  <input v-model="input" placeholder="Maslo" /><br />
  <button @click="addItem">Pridať</button>

  <p style="margin-top: 0">______________________________________</p>

  <h2>Polozky</h2>
  <ul>
    <li v-for="item in validItems" :key="`item-${item.id}`">
      <span @click="deleteItem(item)" style="margin-right: 15px">X</span>
      {{ item.text }}
    </li>
  </ul>

  <p style="margin-top: 0">______________________________________</p>

  <h2>Zmazane polozky</h2>
  <ul>
    <li v-for="item in deletedItems" :key="`item-${item.id}`">
      <span style="text-decoration: line-through; margin-right: 15px">
        {{ item.text }}
      </span>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      input: "",
    };
  },

  computed: {
    validItems() {
      return this.items.filter((item) => !item.is_deleted);
    },

    deletedItems() {
      return this.items.filter((item) => item.is_deleted);
    },
  },

  methods: {
    addItem() {
      if (this.input.trim()) {
        this.items.push({
          id: this.items.length + 1,
          text: this.input,
          is_deleted: false,
        });
        this.input = "";
      }
    },
    deleteItem(item) {
      item.is_deleted = true;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
