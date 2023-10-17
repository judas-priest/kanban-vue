<template>
<div class="modal2">
  <div class="create-product modal-content">
    <h2 class="create-product-title">Новая карточка</h2>
    <input
      type="text"
      v-model="user"
      class="product-input"
      placeholder="Пользователь"
    />
    <input
      type="text"
      v-model="phone"
      class="product-input"
      placeholder="Номер телефона"
    />
    <input
      type="text"
      v-model="email"
      class="product-input"
      placeholder="Электронная почта"
    />
        <input
      type="text"
      v-model="price"
      class="product-input"
      placeholder="Цена"
    />
        <input
      type="text"
      v-model="name"
      class="product-input"
      placeholder="Название"
    />
    <button class="create-button" @click="createProduct">Создать</button>
  </div>
  </div>
</template>

<script>

import axios from 'axios';
export default {
  data() {
    return {
      product: {
        user: '',
        phone: '',
        email: '',
        price: '',
        name: ''
      },
    };
  },
  methods: {
    createProduct() {
      const data = {
        user: this.user,
        phone: this.phone,
        email: this.email,
        price: this.price,
        name: this.name
      };


       axios({
        method: "post",
        url: "http://localhost/sq/Barybians-PHP-REST/v3/index.php?r=Products",
        data: data,
        headers: {
          "Content-Type": "application/x-www-form-urlencoded"
        },
      })




    },
  },
};
</script>

<style scoped>
.create-product {
  background-color: #fafafa;
  padding: 1rem;
  border-radius: 4px;
}

.create-product-title {
  margin-bottom: 1rem;
  font-size: 1.2rem;
  font-weight: bold;
}

.product-input {
  margin-bottom: 0.5rem;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.create-button {
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 0.5rem 1rem;
  font-size: 1rem;
  cursor: pointer;
}
.modal2 {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 2rem;
}
</style>