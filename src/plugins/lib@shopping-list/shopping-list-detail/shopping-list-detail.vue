<template>
  <div v-if="shoppingList">
    <div class="horizontal-align">
      <h1>{{ shoppingList.title }}</h1>
      <button @click="deleteCheckedItems" class="delete-button">Delete</button>
    </div>
    <ul>
      <template
        v-if="
          shoppingList.items &&
          Array.isArray(shoppingList.items) &&
          shoppingList.items.length > 0
        "
      >
        <li v-for="item in shoppingList.items" :key="item.id">
          <div class="light">
            <input
              class="checkbox"
              type="checkbox"
              v-model="item.is_checked"
              @change="updateItemChecked(item)"
            />
            {{ item.name }}
          </div>
          <div class="blue-background">{{ item.value }} {{ item.unit }}</div>
        </li>
      </template>
      <template v-else>
        <li class="grey-color">Žiadne položky v zozname.</li>
      </template>
      <input
        type="text"
        v-model="newItemName"
        placeholder="New item"
        @keyup.enter="addNewItem"
        class="new-item-input"
      />
    </ul>
  </div>

  <div v-else>
    <p>Načítavam zoznam...</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      shoppingList: null,
    };
  },

  async mounted() {
    try {
      const {
        data: { data: shoppingLists },
      } = await axios.get("/api/v1/shopping-lists");
      this.shoppingList = shoppingLists.find(
        ({ id }) => id == this.$route.params.id
      );
      console.log(this.shoppingList);
    } catch (error) {
      console.error("Error:", error);
      this.shoppingList = { error };
    }
  },

  methods: {
    async deleteCheckedItems() {
      try {
        const checkedItems = this.shoppingList.items.filter(
          (item) => item.is_checked
        );

        console.log(checkedItems);
        for (const item of checkedItems) {
          await axios.delete(
            `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}/items/${item.id}`
          );
          console.log(`Deleted item: ${item.name}`);
        }

        this.shoppingList.items = this.shoppingList.items.filter(
          (item) => !item.is_checked
        );
      } catch (error) {
        console.error("Error deleting items:", error.message);
      }
    },

    async updateItemChecked(item) {
      try {
        await axios.put(
          `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}/items/${item.id}`,
          {
            is_checked: item.is_checked,
          }
        );
        console.log(this.shoppingList);
      } catch (error) {
        console.error(error);
      }
    },

    async addNewItem() {
      if (this.newItemName.trim() === "") {
        return;
      }

      try {
        const newItem = {
          name: this.newItemName,
          value: 1,
          unit: "piece",
          is_checked: false,
        };
        const response = await axios.post(
          `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}/items`,
          newItem
        );
        this.shoppingList.items.push(response.data.data);
        this.newItemName = "";
      } catch (error) {
        console.error("Error:", error);
      }
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

.horizontal-align {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.delete-button {
  background-color: transparent;
  color: salmon;
  border: 2px solid red;
  border-radius: 10px;
  height: 5vh;
  width: 10vh;
  font-size: 110%;
}

.delete-button:hover {
  cursor: pointer;
  color: red;
  border: 2px solid red;
  background-color: salmon;
}
</style>
