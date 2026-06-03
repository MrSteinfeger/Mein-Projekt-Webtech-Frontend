<template>
  <div class="shop-container">
    <h2>Unsere Produkte</h2>
    <p v-if="products.length === 0">Lade Produkte aus dem Backend...</p>

    <ul v-else>
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
      products: []
    }
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      try {
        // KORRIGIERT: /api/products hinzugefügt, passend zum Spring Boot @GetMapping
        const response = await fetch('https://webtech-mein-projekt-1.onrender.com/api/products');
        if (!response.ok) {
          throw new Error('Fehler beim Laden der Daten');
        }
        this.products = await response.json();
      } catch (error) {
        console.error("Da ging was schief:", error);
      }
    }
  }
}
</script>

<style scoped>
.shop-container { border: 1px solid #42b983; padding: 20px; border-radius: 8px; margin: 20px; }
li { list-style-type: none; margin-bottom: 10px; font-weight: bold; }
</style>