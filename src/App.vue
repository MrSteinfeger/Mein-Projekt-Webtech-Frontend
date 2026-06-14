<template>
  <div class="shop-container">
    <h2>Neues Produkt hinzufügen</h2>
    <form @submit.prevent="saveProduct" class="product-form">
      <input v-model="newProduct.name" placeholder="Produktname" required />
      <input v-model.number="newProduct.price" type="number" step="0.01" placeholder="Preis in €" required />
      <button type="submit">Produkt speichern</button>
    </form>

    <hr>

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
      products: [],
      // Speicher für das neue Produkt aus dem Formular
      newProduct: {
        name: '',
        price: null
      }
    }
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    // 1. Deine bestehende GET-Methode (schon perfekt umgestellt!)
    async fetchProducts() {
      try {
        const response = await fetch('https://webtech-mein-projekt-1.onrender.com/api/products');
        if (!response.ok) {
          throw new Error('Fehler beim Laden der Daten');
        }
        this.products = await response.json();
      } catch (error) {
        console.error("Da ging was schief:", error);
      }
    },

    // 2. DIE NEUE POST-METHODE (Das verlangt deine Aufgabe!)
    async saveProduct() {
      try {
        const response = await fetch('https://webtech-mein-projekt-1.onrender.com/api/products', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.newProduct) // Schickt Name & Preis an das Spring Backend
        });

        if (!response.ok) {
          throw new Error('Fehler beim Speichern');
        }

        // Nach erfolgreichem Speichern: Liste neu laden und Formular leeren
        await this.fetchProducts();
        this.newProduct.name = '';
        this.newProduct.price = null;
        alert('Produkt erfolgreich in Render-Datenbank gespeichert!');

      } catch (error) {
        console.error("Speichern fehlgeschlagen:", error);
      }
    }
  }
}
</script>

<style scoped>
.shop-container { border: 1px solid #42b983; padding: 20px; border-radius: 8px; margin: 20px; }
.product-form { margin-bottom: 20px; display: flex; gap: 10px; }
input, button { padding: 8px; border-radius: 4px; border: 1px solid #ccc; }
button { background-color: #42b983; color: white; cursor: pointer; font-weight: bold; }
li { list-style-type: none; margin-bottom: 10px; font-weight: bold; }
</style>