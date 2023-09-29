<template>
  <div id="app">
    <h1>Product Management</h1>
    
    <h2>Add Product</h2>
    <form @submit.prevent="addProduct">
      Name: <input v-model="newProduct.name">
      Price: <input v-model="newProduct.price" type="number">
      Description: <textarea v-model="newProduct.description"></textarea>
      <button type="submit">Add Product</button>
    </form>

    <!-- List Products -->
    <h2>Products</h2>
    <div v-for="product in products" :key="product.id">
      <h3>{{ product.name }}</h3>
      <p>Price: ${{ product.price }}</p>
      <p>{{ product.description }}</p>
    </div>
    <div>
      <input v-model="discountCode" placeholder="Enter Discount Code">
      <button @click="applyDiscount">Apply Discount</button>
    </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      newProduct: {
        name: '',
        price: '',
        description: ''
      },
      discountCode: "",
      products: []
    }

  },
 

  methods: {
    addProduct() {
      // Send the new product to the server to save in the database
      fetch('http://localhost/back-end-discount/createProduct.php', {
        
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(this.newProduct),
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();
      })
      .then(data => {
        console.log(data); // Log the response from the server

        // Clear the form
        this.newProduct = {
          name: '',
          price: '',
          description: ''
        };

        // Fetch the updated product list
        this.fetchProducts();
      })
      .catch(error => {
        console.error('There was a problem with the fetch operation:', error);
      });
    },
    fetchProducts() {
      fetch('http://localhost/back-end-discount/getProducts.php')
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then(data => {
          this.products = data;
        })
        .catch(error => {
          console.error('There was a problem with the fetch operation:', error);
        });
    },

    applyDiscount() {
  console.log(this.discountCode)

      fetch('http://localhost/back-end-discount/applyDiscount.php', { 
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(this.discountCode),
      })

      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();

      })
      .then(data => {
        console.log(data)});
        console.log(this.discountCode)
  
    },
  
 
  created() {
    // Fetch the product list when the component is created
    this.fetchProducts();
  },

}}

</script>

<style>
/* Your CSS styles here */
</style>
