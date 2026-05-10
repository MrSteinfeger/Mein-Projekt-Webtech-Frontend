<template>
  <div class="shop-container">
    <h2>Unsere Produkte</h2>
    <ul>
      <li v-for="product in products" :key="product.id">
        {{ product.name }} - {{ product.price }} €
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Laptop', price: 999 },
        { id: 2, name: 'Maus', price: 25 },
        { id: 3, name: 'Tastatur', price: 50 }
      ]
    }
  }
}
</script>

<style scoped>
.shop-container { border: 1px solid #42b983; padding: 20px; border-radius: 8px; margin: 20px; }
li { list-style-type: none; margin-bottom: 10px; font-weight: bold; }
</style>