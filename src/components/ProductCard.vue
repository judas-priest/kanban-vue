<template>
  <div class="product-card">
    <div class="product-data col-12">
    <div class="wrapper">
    <div class="product"><span class="data">ID:</span> {{ product.product_id }}</div>
    <div class="product"><span class="data">Имя:</span> {{ product.name }}</div>
    <div class="product"><span class="data">Пользователь:</span> {{ product.user }}</div>
    <div class="product"><span class="data">E-Mail:</span> {{ product.email }}</div>
    <div class="product"><span class="data">Цена:</span> {{ product.price }} р</div>
    <div class="product"><span class="data">Дата:</span> {{ formattedDate  }}</div>
    </div>
    </div>
    <div class="buttons col-6">
    <button class="btn btn-warning col-12" >Изменить</button> 
    <button class="btn btn-danger col-12" >Удалить</button>
    <button class="btn btn-primary col-12" @click="$emit('toProgress', product)" v-show="product.status === null || product.status === 'products'">В работу</button>
    <button class="btn btn-secondary col-12" @click="$emit('toDone', product)" v-show="product.status === 'progressProducts'">Завершить</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
  methods: {
  },
    computed: {
    formattedDate() {
      const date = new Date(this.product.date * 1000);
      return ("0" + date.getDate()).slice(-2) + "." +  ("0" + (date.getMonth() + 1)).slice(-2) + "." + date.getFullYear() + " " + ("0" + date.getHours()).slice(-2) + ":" + ("0" + date.getMinutes()).slice(-2)
    },
  },
};
</script>

<style scoped>

.product-card {
  background-color: #fafafa;
  border-radius: 4px;
  padding: 1rem;
  margin-bottom: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.product-data{
  margin-bottom: 1rem;
}
.data{
  font-weight: bold!important;
}
.buttons>span{
}

</style>