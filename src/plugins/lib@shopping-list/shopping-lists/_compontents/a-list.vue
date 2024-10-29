<template>
  <ul>
    <template v-if="list.items && list.items.length">
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
</template>

<script>
export default {
  props: {
    list: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped></style>
