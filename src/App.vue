<template>
  <div class="container">
    <!-- Category Section -->
    <div v-for="(category, index) in categories" :key="index" class="category_list" role="list">
      <CategoryComponent
        :title="category.name"
        :itemCounts="category.productCount"
        :bg_color="category.color"
        :image_path="category.image"
      />
    </div>

    <!-- Promotions Section -->
    <div v-for="(promotion, index) in promotions" :key="index" class="poster_list">
      <PromotionComponent
        :title="promotion.title"
        :image_path="promotion.image"
        :bgColor="promotion.color"
        :btn_Color="promotion.buttonColor"
      />
    </div>
  </div>
</template>

<script lang="ts">
import CategoryComponent from './components/CategoryComponent.vue'
import PromotionComponent from './components/PromotionComponent.vue'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    CategoryComponent,
    PromotionComponent,
  },
  data() {
    return {
      promotions: [],
      categories: [],
    }
  },

  methods: {
    async fetchPromotions() {
      try {
        const response = await axios.get('http://localhost:3000/api/promotions')
        this.promotions = response.data
        console.log('Promotions fetched:', this.promotions)
      } catch (error) {
        console.error('Error fetching promotions:', error)
      }
    },

    async fetchCategories() {
      try {
        const response = await axios.get('http://localhost:3000/api/categories')
        this.categories = response.data
        console.log('Categories fetched:', this.categories)
      } catch (error) {
        console.error('Error fetching categories:', error)
      }
    },
  },

  mounted() {
    this.fetchPromotions()
    this.fetchCategories()
  },
}
</script>

<style scoped>
/* Container */
.container {
  width: 100%;
  height: 100%;
  display: flex;
  gap: 32px;
  padding: 24px;
  box-sizing: border-box;
}

/* Horizontal Scroll for Categories */
.category_list {
  display: flex;
  flex-direction: row;
  overflow-x: auto;
  scroll-behavior: smooth;
  gap: 16px;
  flex-wrap: nowrap;
  padding-bottom: 8px;
  -webkit-overflow-scrolling: touch;
  scroll-snap-type: x mandatory;
}
.category_list > * {
  flex-shrink: 0;
  min-width: 160px; /* adjust as needed */
  scroll-snap-align: start;
}

/* Horizontal Scroll for Promotions */
.poster_list {
  display: flex;
  flex-direction: row;
  overflow-x: auto;
  scroll-behavior: smooth;
  gap: 24px;
  flex-wrap: nowrap;
  padding-bottom: 8px;
  -webkit-overflow-scrolling: touch;
  scroll-snap-type: x mandatory;
}
.poster_list > * {
  flex-shrink: 0;
  min-width: 300px; /* adjust per card size */
  scroll-snap-align: start;
}

/* Optional: hide scrollbar for both sections */
.category_list::-webkit-scrollbar,
.poster_list::-webkit-scrollbar {
  display: none;
}
</style>
