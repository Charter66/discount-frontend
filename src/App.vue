<template>
  <div class="app">
  <div class="addProduct">
    <h1>Product Management</h1>
    
    <h2>Add Product</h2>
    <form @submit.prevent="addProduct">
      Name: <input v-model="newProduct.name">
      Price: <input v-model="newProduct.price" type="number">
      Description: <textarea v-model="newProduct.description"></textarea>
      <button type="submit">Add Product</button>
    </form>
    </div>

  <!-- List Products -->
  <h2>Products</h2>
    <div class="product-grid">
      <div v-for="product in products" :key="product.id" class="product-card">
        <h3>{{ product.name }}</h3>
        <p>Price: ${{ product.price }}</p>
        <p>{{ product.description }}</p>
        <button @click="addToCart(product)">Add To Cart</button>
      </div>
    </div>
    <CartOne v-if="cartProducts.length > 0" :items="cartProducts" />
  </div>
</template>

<script>
import CartOne from './components/Cart'
export default {
  components: { 
    CartOne
  },
  data() {
    return {
      newProduct: {
        name: '',
        price: '',
        description: ''
      },
      discountCode: "",
      products: [],
      cartProducts: [],
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

    addToCart(product){
      console.log(product)
      this.cartProducts.push(product)
     
    }
  },

  mounted() {
    // Fetch the product list when the component is mounted
    this.fetchProducts();
  },
}
</script>

<style>
.app{
  background: lightgray;

}
.addProduct {
  font-family: Arial, sans-serif;
  max-width: 500px;
  margin: 30px;
  padding: 0px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

h2 {
  font-size: 20px;
  margin-top: 30px;
  margin-bottom: 10px;
}

form {
  margin-bottom: 20px;
}

form input,
form textarea {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button[type="submit"],
button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button[type="submit"]:hover,
button:hover {
  background-color: #0056b3;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Exactly 3 columns */
  grid-gap: 20px; /* Adjust the gap between products */
  margin :30px;
}


.product-card {
  border: 1px solid #e6e6e6;  /* Slightly off-white for subtlety */
  border-radius: 5px;
  padding: 10px;
  background-color: #ffffff; /* A clean white background */
  
  /* Box Shadow */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.12);
  
  /* Transition for hover effect */
  transition: box-shadow 0.3s ease, transform 0.3s ease;
  
  /* Optional: Slight scale on hover for interactive feeling */
&:hover {
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15), 0 2px 5px rgba(0, 0, 0, 0.1);
    transform: scale(1.02);
  }
}

.product-container {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 20px;

}

.product-container h3 {
  font-size: 18px;
  margin: 0;
}

.product-container p {
  font-size: 16px;
  margin: 10px 0;
}

.input-container {
  margin-top: 20px;
  display: flex;
  align-items: center;
}

.input-container input {
  width: 70%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

.input-container button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.input-container button:hover {
  background-color: #0056b3;
}
</style>

