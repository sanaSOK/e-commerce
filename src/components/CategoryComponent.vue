<template>
  <button
    role="menuitem"
    type="button"
    class="category_btn"
    :style="{ backgroundColor: bg_color }"
  >
    <img class="category_img" :src="`http://localhost:3000/${image_path}`" alt="Category image" />
    <span class="category_name">{{ title }}</span>
    <span class="category_count">{{ itemCounts }} items</span>
  </button>
</template>

<script lang="ts">
export default {
  name: 'CategoryComponent',
  props: {
    image_path: String,
    itemCounts: Number,
    title: String,
    bg_color: String,
  },
}
</script>

<style scoped>
.category_btn {
  padding: 5px;
  min-width: 100px;
  width: 100px;
  height: 140px;
  margin: 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: var(--bg);
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 8px;
}

.category_btn:hover {
  background: var(--hover);
}

.category_img {
  width: 65%;
  margin-bottom: 10px;
}

.category_name {
  
  font-size: 11px;
  font-weight: bold;
  margin-bottom: 5px;
}

.category_count {
  font-size: 10px;
  color: lightslategray;
}
</style>
