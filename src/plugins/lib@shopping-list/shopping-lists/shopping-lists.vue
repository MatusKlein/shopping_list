<template>
  <h1>Lists</h1>

  <template v-if="!shoppingLists">
    <p>Načítavam dáta</p>
  </template>

  <template v-else-if="shoppingLists.error">
    <p>Pri načítaní dát nastala chyba: {{ shoppingLists.error }}</p>
  </template>

  <template v-else>
    <p class="grey-color">
      Počet položiek v zozname: {{ shoppingLists.length }}
    </p>

    <div
      v-for="(list, key) in shoppingLists"
      :key="key"
      class="shopping-list-card"
    >
      <a
        :href="`/shopping-lists/${list.id}`"
        @click.prevent="openShoppingListDetail(list)"
      >
        <h2>{{ list.title }}</h2>
      </a>
      <ul>
        <template
          v-if="
            list.items && Array.isArray(list.items) && list.items.length > 0
          "
        >
          <li v-for="item in list.items.slice(0, 3)" :key="item.id">
            <div class="light">
              {{ item.name }}
            </div>
            <div class="blue-background">{{ item.value }} {{ item.unit }}</div>
          </li>
        </template>
        <template v-else>
          <li class="grey-color">Žiadne položky v zozname.</li>
        </template>
      </ul>
    </div>
  </template>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      shoppingLists: null,
    };
  },

  async mounted() {
    try {
      // const response = await axios.get('https://shoppinglist.wezeo.dev/shoppinglist/lists')
      // const data = response.data.data

      // Skrateny zapis zakomentovaneho kodu vyssie, kde vytiahneme data pomocou destrukcie objektu
      const {
        data: { data: shoppingLists },
      } = await axios.get("/api/v1/shopping-lists");
      this.shoppingLists = shoppingLists;
      console.log("Shopping Lists:", this.shoppingLists);
    } catch (error) {
      console.error("Error:", error);
      this.shoppingLists = { error };
    }
  },

  methods: {
    openShoppingListDetail({ id }) {
      this.$router.push({ name: "Shopping List - Detail", params: { id } });
    },
  },
};
</script>

<style>
html {
  background-color: black;
  padding: 0vw 2vw 0vw 2vw;
}

#app {
  text-align: left;
}

h1,
p {
  color: white;
}

h2 {
  color: white;
  padding-bottom: 2vw;
  font-size: 3.5vh;
}

.shopping-list-card {
  border-radius: 25px;
  padding: 1vw 3vw 1vw 3vw;
  margin-bottom: 1vw;
  background-color: #1a1a1a;
}

.blue-background {
  background-color: #007fff;
  padding: 0.3vw 1vw 0.2vw 1vw;
  border-radius: 25px;
  font-family: Arial, sans-serif;
}

.grey-color {
  color: grey;
}

li {
  justify-content: space-between;
  color: white;
  display: flex;
  margin-bottom: 1vw;
  font-family: "Arial Narrow", Arial, sans-serif;
}

ul {
  padding: 0;
}

a {
  text-decoration: none;
}
</style>
