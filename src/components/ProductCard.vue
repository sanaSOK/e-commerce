<template>
  <div class="product-card">
    <div class="badge" v-if="badge" :class="badgeClass">{{ badge }}</div>

    <div class="img-wrap">
      <img
        class="product-img"
        :src="`http://localhost:3000/${image}`"
        alt="product"
        loading="lazy"
        @error="onImageError"
      />
    </div>

    <div class="product-body">
      <div class="store-name">{{ store || 'Hodo Foods' }}</div>
      <div class="product-title">{{ title }}</div>

      <div class="rating-row">
        <div class="stars">{{ starString }}</div>
        <div class="rating-count">({{ rating.toFixed(1) }})</div>
      </div>

      <div class="product-meta">{{ weight }}</div>

      <div class="product-bottom">
        <div class="price-block">
          <div>
            <span class="price">${{ price }}</span>
            <span v-if="oldPrice" class="old-price">${{ oldPrice }}</span>
          </div>
        </div>

        <div class="actions">
          <div class="qty" v-if="showQty">
            <button @click="decrement">-</button>
            <input type="number" v-model.number="quantity" min="1" />
            <button @click="increment">+</button>
          </div>
          <button class="add-btn">Add +</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'ProductCard',
  props: {
    title: String,
    price: Number,
    oldPrice: Number,
    weight: Number,
    image: String,
    badge: String,
    store: String,
    rating: Number,
    showQty: Number
  },
  data() {
    return { quantity: 1 }
  },
  computed: {
    resolvedImage() {
      const image = this.image || ''
      if (image.startsWith('http') || image.startsWith('/')) return image
      return `http://localhost:3000/${image}`
    },
    badgeClass() {
      if (!this.badge) return ''
      const b = String(this.badge).toLowerCase()
      if (b.includes('hot')) return 'badge-hot'
      if (b.includes('sale')) return 'badge-sale'
      if (b.includes('%') || b.includes('-')) return 'badge-discount'
      return 'badge-default'
    },
    starString() {
      const full = Math.round(this.rating)
      return '★'.repeat(full) + '☆'.repeat(Math.max(0, 5 - full))
    },
  },
  methods: {
    // onImageError(event) {
    //   event.target.src = 'https://media.istockphoto.com/id/814423752/photo/eye-of-model-with-colorful-art-make-up-close-up.jpg?s=612x612&w=0&k=20&c=l15OdMWjgCKycMMShP8UK94ELVlEGvt7GmB_esHWPYE='
    // },
    increment() {
      this.quantity += 1
    },
    decrement() {
      if (this.quantity > 1) this.quantity -= 1
    },
  },
}
</script>

<style scoped>
.product-card {
  background: #ffffff;
  border-radius: 12px;
  border: 1px solid rgba(46, 204, 113, 0.08);
  box-shadow: 0 6px 18px rgba(16, 62, 53, 0.03);
  padding: 12px;
  box-sizing: border-box;
  position: relative;
  display: flex;
  flex-direction: column;
  height: 420px; /* fixed height for horizontal scroller */
}
.img-wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 10px;
  padding-bottom: 6px;
  height: 160px; /* fixed image area */
}
.product-img {
  max-width: 180px;
  max-height: 140px;
  width: auto;
  height: auto;
  object-fit: contain;
  background: transparent;
}
.product-body {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 6px 8px;
}
.store-name {
  font-size: 12px;
  color: #00ffdd;
}
.product-title {
  font-size: 13px;
  font-weight: 600;
  color: #0051ff;
  line-height: 1.2;
  display: -webkit-box;
  line-clamp: 2;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.rating-row {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 12px;
  color: #f6a623;
}
.stars {
  color: #f6a623;
}
.rating-count {
  color: #97a6a5;
  font-size: 12px;
}
.product-meta {
  font-size: 12px;
  color: #00f0e0;
}
.product-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: auto; /* push to bottom */
}
.price {
  color: #23b574;
  font-weight: 700;
  margin-right: 8px;
  font-size: 16px;
}
.old-price {
  color: #97a6a5;
  text-decoration: line-through;
  margin-left: 8px;
  font-size: 12px;
}
.actions {
  display: flex;
  align-items: center;
  gap: 8px;
}
.qty {
  display: flex;
  align-items: center;
  gap: 6px;
}
.qty button {
  width: 26px;
  height: 26px;
  border-radius: 6px;
  background: #f1f8f5;
  border: 1px solid #d6f3e2;
}
.qty input {
  width: 44px;
  text-align: center;
  border: 1px solid #e6efed;
  border-radius: 6px;
  padding: 4px;
}
.add-btn {
  background: #e8faf0;
  border: 1px solid #d6f3e2;
  color: #2a9d66;
  padding: 6px 10px;
  border-radius: 8px;
  cursor: pointer;
}
.badge {
  position: absolute;
  top: 12px;
  left: 12px;
  padding: 6px 10px;
  border-radius: 12px;
  font-size: 12px;
  color: #fff;
}
.badge-hot {
  background: #ff6b6b;
}
.badge-sale {
  background: #f6a623;
}
.badge-discount {
  background: #2ecc71;
}
.badge-default {
  background: #6b7280;
}

.product-card:hover {
  transform: translateY(-4px);
  transition: transform 120ms ease;
}
</style>
