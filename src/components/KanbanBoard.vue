<template>

    <CreateProduct v-if="create" createProduct="createProduct" />
  <div class="kanban-board">
    <div class="columns">
      <div class="column" data-id="products"
      @dragover.prevent
      @dragenter.prevent
      @drop="onDrop($event)">
        <h2 class="column-title">Необработанные</h2>
        <button class="btn btn-secondary" :disabled="products == ''" @click="products.sort((a, b) => a.product_id - b.product_id)">Сортировать</button>
        <button class="btn btn-success" @click="create = true">Создать</button>
        <div class="card-list to--do">
          <ProductCard
            v-for="product in products"
            :key="product.product_id"
            :product="product"
            @toProgress="toProgress"
            @toDone="toDone"
             @dragstart="onDragStart($event, product.product_id, 'products')"
             draggable="true"
          />
        </div>
      </div>
      <div class="column" data-id="progressProducts"
      @dragover.prevent
      @dragenter.prevent
      @drop="onDrop($event)">
        <h2 class="column-title">В работе</h2>
        <button class="btn btn-secondary" :disabled="progressProducts == ''" @click="progressProducts.sort((a, b) => a.product_id - b.product_id)">Сортировать</button>
        <div class="card-list in--progress">
          <ProductCard
            v-for="product in progressProducts"
            :key="product.product_id"
            :product="product"
            @toProgress="toProgress"
            @toDone="toDone"
            @dragstart="onDragStart($event, product.product_id, 'progressProducts')"
             draggable="true"
          />
        </div>
      </div>
      <div class="column" data-id="doneProducts"
      @dragover.prevent
      @dragenter.prevent
      @drop="onDrop($event)">
        <h2 class="column-title">Завершённые</h2>
        <button class="btn btn-secondary" :disabled="doneProducts == ''" @click="doneProducts.sort((a, b) => a.product_id - b.product_id)">Сортировать</button>
        <div class="card-list done">
          <ProductCard
            v-for="product in doneProducts"
            :key="product.product_id"
            :product="product"
            @toProgress="toProgress"
            @toDone="toDone"
             @dragstart="onDragStart($event, product.product_id, 'doneProducts')"
             draggable="true"
          />
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import ProductCard from './ProductCard.vue';
import CreateProduct from './CreateProduct.vue';

export default {
  components: {
    ProductCard,
    CreateProduct,
  },

  data() {
    return {
      products: [],
      progressProducts: [],
      doneProducts: [],
      dragId: '',
      dragCol: '',
      create: false
    };
  },

  created() {
    this.fetchProducts();
  },

  methods: {
    fetchProducts() {
      axios.get('http://localhost/sq/Barybians-PHP-REST/v3/index.php?r=Products')
        .then((response) => {
          this.products = response.data;
        })
        .catch((error) => {
          console.log('Error fetching products:', error);
        });
    },
    onDragStart(event, id, col) {
    console.log(this.doneProducts)
    this.dragId = id
    this.dragCol = col

},
    onDrop(event){
      var  toColumn 
      

if(event.target.dataset.id){
  toColumn = event.target.dataset.id
}

else{
  let currentElement = event.target;

  while (currentElement.parentNode) {
  
    currentElement = currentElement.parentNode;
    if (currentElement.dataset.id) {
      console.log("Found a dog!");
       toColumn = currentElement.dataset.id
      break;
    }
  }
    }
    const fromColumn = this.dragCol
    const id = this.dragId



console.log(toColumn + ' ' + fromColumn + ' ' + id)

if(toColumn && fromColumn && id){
  const index = this[fromColumn].findIndex((p) => p.product_id === id);

    this[fromColumn][index].status = toColumn
    const product = this[fromColumn].splice(index, 1)[0];

    this[toColumn].push(product)

 
}
    },
    toProgress(e){
      const index = this.products.findIndex((p) => p.product_id === e.product_id);

    e.status = 'progressProducts'
    const product = this.products.splice(index, 1)[0];

    this.progressProducts.push(product)
    },
        toDone(e){
        console.log('HUIHUIHUIHUI')
      const index = this.progressProducts.findIndex((p) => p.product_id === e.product_id);

    e.status = 'doneProducts'
    const product = this.progressProducts.splice(index, 1)[0];

    this.doneProducts.push(product)
    }
  },
  sortProducts(){
  console.log('myau')
  this.products.sort((a, b) => a.id - b.id)
  
  }

};
</script>

<style scoped>

.columns {
  display: flex;
  justify-content: space-between;
  border-radius: 25px;
}
@media screen and (max-width: 700px) {
  .columns {
    display: block;
    width: 100%;
  }
}
.column {
  flex: 1;
  margin: 1rem 1rem;
}

.column-title {
  margin-bottom: 1rem;
  text-align: center;
}

.card-list {
  border-radius: 4px;
}

.kanban-board {
  margin: 0 auto;
}
.to--do:not(:empty){
  border-radius: 4px;
  border: 1rem solid #70a0ff;
  background-color: #70a0ff;
}
.in--progress:not(:empty){
  border-radius: 4px;
  border: 1rem solid #ffff90;
  background-color:  #ffff90;
}
.done:not(:empty){
  border-radius: 4px;
  border: 1rem solid #70ffa0;
  background-color:  #70ffa0;
}


.droppable {
  padding: 15px;
  border-radius: 5px;
  background: #2c3e50;
  margin-bottom: 10px;
}

.droppable h4 {
  color: white;
}

.draggable {
  background: white;
  padding: 5px;
  border-radius: 5px;
  margin-bottom: 5px;
}

.draggable h5 {
  margin: 0;
}
</style>
