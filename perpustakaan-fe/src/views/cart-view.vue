<template>
  <div class="cart">
    <h1>Cart</h1>

    <!-- Form untuk menambahkan item baru ke cart -->
    <form @submit.prevent="addCartItem" class="cart-form">
      <div class="form-group">
        <label for="title">Title:</label>
        <input type="text" id="title" v-model="newCartItem.title" required />
      </div>
      <div class="form-group">
        <label for="quantity">Quantity:</label>
        <input
          type="number"
          id="quantity"
          v-model="newCartItem.quantity"
          required
        />
      </div>
      <div class="form-group">
        <label for="days">Days:</label>
        <input type="number" id="days" v-model="newCartItem.days" required />
      </div>
      <button type="submit" class="btn btn-primary">Add Item</button>
    </form>

    <!-- List item di cart -->
    <ul class="cart-list">
      <li v-for="item in cartItems" :key="item.id" class="cart-item">
        <span class="item-info">
          {{ item.title }} ({{ item.quantity }} units for {{ item.days }} days)
        </span>
        <div class="item-actions">
          <button class="btn btn-secondary" @click="editCartItem(item)">
            Edit
          </button>
          <button class="btn btn-danger" @click="deleteCartItem(item.id)">
            Delete
          </button>
        </div>
      </li>
    </ul>

    <!-- Modal untuk mengedit item -->
    <div v-if="isEditing" class="modal">
      <form @submit.prevent="updateCartItem" class="modal-form">
        <h2>Edit Item</h2>
        <div class="form-group">
          <label for="edit-title">Title:</label>
          <input
            type="text"
            id="edit-title"
            v-model="currentCartItem.title"
            required
          />
        </div>
        <div class="form-group">
          <label for="edit-quantity">Quantity:</label>
          <input
            type="number"
            id="edit-quantity"
            v-model="currentCartItem.quantity"
            required
          />
        </div>
        <div class="form-group">
          <label for="edit-days">Days:</label>
          <input
            type="number"
            id="edit-days"
            v-model="currentCartItem.days"
            required
          />
        </div>
        <div class="modal-actions">
          <button type="submit" class="btn btn-primary">Save</button>
          <button
            type="button"
            class="btn btn-secondary"
            @click="isEditing = false"
          >
            Cancel
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      cartItems: [],
      newCartItem: {
        title: "",
        quantity: 0,
        days: 0,
      },
      isEditing: false,
      currentCartItem: null,
    };
  },
  methods: {
    // Fetch cart items
    async fetchCartItems() {
      try {
        const response = await axios.get("http://127.0.0.1:8000/api/cart");
        this.cartItems = response.data;
      } catch (error) {
        console.error("Failed to fetch cart items:", error);
      }
    },

    // Add a new cart item
    async addCartItem() {
      try {
        const response = await axios.post(
          "http://127.0.0.1:8000/api/cart",
          this.newCartItem
        );
        this.cartItems.push(response.data);
        this.newCartItem = { title: "", quantity: 0, days: 0 };
      } catch (error) {
        console.error("Failed to add cart item:", error);
      }
    },

    // Edit an existing cart item
    editCartItem(item) {
      this.currentCartItem = { ...item };
      this.isEditing = true;
    },

    // Update a cart item
    async updateCartItem() {
      try {
        const response = await axios.put(
          `http://127.0.0.1:8000/api/cart/${this.currentCartItem.id}`,
          this.currentCartItem
        );
        const index = this.cartItems.findIndex(
          (item) => item.id === this.currentCartItem.id
        );
        if (index !== -1) this.cartItems.splice(index, 1, response.data);
        this.isEditing = false;
        this.currentCartItem = null;
      } catch (error) {
        console.error("Failed to update cart item:", error);
      }
    },

    // Delete a cart item
    async deleteCartItem(id) {
      try {
        await axios.delete(`http://127.0.0.1:8000/api/cart/${id}`);
        this.cartItems = this.cartItems.filter((item) => item.id !== id);
      } catch (error) {
        console.error("Failed to delete cart item:", error);
      }
    },
  },
  mounted() {
    this.fetchCartItems();
  },
};
</script>

<style>
.cart {
  max-width: 600px;
  margin: auto;
  font-family: Arial, sans-serif;
  color: #333;
}

.cart-form {
  margin-bottom: 20px;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.form-group {
  margin-bottom: 10px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.btn {
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-primary {
  background-color: #007bff;
  color: white;
}

.btn-secondary {
  background-color: #6c757d;
  color: white;
}

.btn-danger {
  background-color: #dc3545;
  color: white;
}

.cart-list {
  list-style-type: none;
  padding: 0;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
  background-color: #fff;
}

.item-info {
  flex: 1;
}

.item-actions {
  display: flex;
  gap: 10px;
}

.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  border: 1px solid #ccc;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 1000;
  border-radius: 5px;
  max-width: 400px;
  width: 100%;
}

.modal-form {
  display: flex;
  flex-direction: column;
}

.modal-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}
</style>
