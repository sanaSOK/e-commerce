<template>
  <div>
    <main class="container page-content">
      <div class="categories-header">
        <h2>Featured Categories</h2>
        <div class="categories-links">
          <a
            v-for="(link, idx) in categoryLinks"
            :key="'catlink-' + idx"
            :class="{ active: selectedCategory === link }"
            href="#"
            @click.prevent="selectCategory(link)"
          >
            {{ link }}
          </a>
        </div>
      </div>

      <div class="category_area">
        <div class="category_list" role="list">
          <CategoryComponent
            v-for="(category, index) in categories"
            :key="index"
            :title="category.name"
            :itemCounts="category.productCount"
            :bg_color="category.color"
            :image_path="category.image"
          />
        </div>
      </div>

      <div class="promotions_area">
        <div class="poster_list">
          <PromotionComponent
            v-for="(promotion, index) in promotions"
            :key="index"
            :title="promotion.title"
            :image_path="promotion.image"
            :bgColor="promotion.color"
            :btn_Color="promotion.buttonColor"
          />
        </div>
      </div>

      <section class="popular_section">
        <div class="categories-header">
          <h2>Popular Products</h2>
          <div class="categories-links">
            <a
              v-for="(link, idx) in categoryLinks"
              :key="'poplink-' + idx"
              :class="{ active: selectedCategory === link }"
              href="#"
              @click.prevent="selectCategory(link)"
            >
              {{ link }}
            </a>
          </div>
        </div>

        <div class="product_grid">
          <ProductCard
            v-for="(p, idx) in filteredProducts"
            :key="idx"
            :title="p.name"
            :price="p.price"
            :oldPrice="p.oldPrice"
            :weight="p.weight"
            :image="p.image"
            :badge="String(p.badge || '')"
            :store="p.store"
            :rating="p.rating || 4"
            :showQty="idx === 0"
          />
        </div>
      </section>
    </main>
  </div>
</template>

<script lang="ts">
import CategoryComponent from './components/CategoryComponent.vue'
import PromotionComponent from './components/PromotionComponent.vue'
import ProductCard from './components/ProductCard.vue'
import axios from 'axios'

type Product = {
    title: String,
    price: Number,
    oldPrice: Number,
    weight: Number,
    image: string[],
    badge: String,
    store: String,
    rating: Number,
    showQty: Number
}


export default {
  name: 'App',
  components: {
    CategoryComponent,
    PromotionComponent,
    ProductCard,
  },
  data() {
    return {
      promotions: [],
      categories: [],
      selectedCategory: 'All',
      products: [],
    }
  },

  computed: {
    categoryLinks() {
      if (this.categories && this.categories.length) {
        return ['All', ...this.categories.map((c) => c.name)]
      }
      return [
        'All',
        'Milks & Dairies',
        'Coffes & Teas',
        'Pet Foods',
        'Meats',
        'Vegetables',
        'Fruits',
      ]
    },

    filteredProducts() {
      if (!this.selectedCategory || this.selectedCategory === 'All') return this.products
      return this.products.filter((p) => p.category === this.selectedCategory)
    },
  },

  methods: {
    selectCategory(link) {
      this.selectedCategory = link
    },

    async fetchPromotions() {
      try {
        const response = await axios.get('http://localhost:3000/api/promotions')
        this.promotions = response.data
      } catch (error) {
        console.error('Error fetching promotions:', error)
      }
    },

    async fetchCategories() {
      try {
        const response = await axios.get('http://localhost:3000/api/categories')
        this.categories = response.data
      } catch (error) {
        console.error('Error fetching categories:', error)
      }
    },

    async fetchProducts() {
      try {
        const response = await axios.get('http://localhost:3000/api/products')
        this.products = response.data
        for (let i = 0; i < this.products.length; i++){


          let temp =`${ this.products[i].image}`;
          temp = temp.replace('["', "");
          temp = temp.replace('"]', "");

          console.log("Image : " + temp);

          this.products[i].image = temp;
          // Ensure an oldPrice exists so ProductCard always displays it
          const priceNum = Number(this.products[i].price) || 0
          if (!this.products[i].oldPrice && priceNum > 0) {
            // default old price = 15% higher than current price
            this.products[i].oldPrice = Math.round(priceNum * 1.15 * 100) / 100
          }
        }


      } catch (error) {
        console.error('Error fetching categories:', error)
      }
    },

  },

  mounted() {
    this.fetchPromotions()
    this.fetchCategories()
    this.fetchProducts()
  },
}
</script>

<style scoped>
/* Container: This is correct for stacking the two sections */
.container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column; /* Stacks Category Area on top of Promotions Area */
  gap: 24px; /* Space between the two main areas */
  padding: 24px;
  box-sizing: border-box;
}

/* New: Area for Categories (to hold the category_list) */
.category_area {
  width: 100%;
}

/* Header row above categories: title left, small links right */
.categories-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin: 8px 0 16px;
}
.categories-header h2 {
  margin: 0;
  font-size: 22px;
  color: #17323a;
}
.categories-links {
  display: flex;
  gap: 18px;
  align-items: center;
  font-size: 13px;
  color: #7f9b99;
}
.categories-links a {
  color: inherit;
  text-decoration: none;
}
.categories-links a.active {
  color: #17323a;
  font-weight: 700;
}

/* Promotions Area (to hold the poster_list) */
.promotions_area {
  width: 100%;
}

.category_list {
  display: flex;
  flex-direction: row;
  overflow-x: auto;
  scroll-behavior: smooth;
  gap: 16px;
  flex-wrap: nowrap;
  padding: 8px 0;
  -webkit-overflow-scrolling: touch;
  scroll-snap-type: x mandatory;
}
.category_list > * {
  flex-shrink: 0;
  min-width: 100px;
  max-width: 120px;
  scroll-snap-align: start;
}

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
  min-width: 300px;
  scroll-snap-align: start;
}

.category_list::-webkit-scrollbar,
.poster_list::-webkit-scrollbar {
  display: none;
}

.page-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px;
  box-sizing: border-box;
}
.popular_section {
  width: 100%;
  margin-top: 12px;
  padding-bottom: 48px;
}
.product_grid {
  display: flex;
  flex-direction: row;
  overflow-x: auto;
  scroll-behavior: smooth;
  gap: 18px;
  flex-wrap: nowrap;
  padding: 8px 0;
  -webkit-overflow-scrolling: touch;
  scroll-snap-type: x mandatory;
}
.product_grid > * {
  flex-shrink: 0;
  min-width: 240px;
  max-width: 240px;
  scroll-snap-align: start;
}
.product_grid::-webkit-scrollbar {
  display: none;
}
</style>
