<template>
  <li>
    <div class="light">
      <input
        class="checkbox"
        type="checkbox"
        :checked="item.is_checked"
        @change="handleCheckboxChange"
      />
      {{ item.name }}
    </div>
    <div class="blue-background">{{ item.value }} {{ item.unit }}</div>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  methods: {
    handleCheckboxChange(event) {
      this.$emit("update-item-checked", {
        id: this.item.id,
        isChecked: event.target.checked,
      });
    },
  },
};
</script>

<style scoped></style>
