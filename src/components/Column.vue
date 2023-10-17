<template>
  <div class="column">
    <h2 class="column-title">{{ title }}</h2>
    <div class="card-list">
      <ProductCard
        v-for="card in cards"
        :key="card.id"
        :card="card"
        @delete-card="deleteCard(card.id)"
      />
    </div>
    <div class="add-card">
      <input type="text" v-model="newCardTitle" class="card-input" placeholder="Enter card title">
      <button class="add-button" @click="addCard">Add Card</button>
    </div>
  </div>
</template>

<script>
import ProductCard from './ProductCard.vue';

export default {
  props: {
    title: {
      type: String,
      required: true,
    },
    cards: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      newCardTitle: '',
    };
  },
  methods: {
    deleteCard(cardId) {
      this.$emit('delete-card', cardId);
    },
    addCard() {
      if (this.newCardTitle) {
        const newCard = {
          id: Date.now(),
          title: this.newCardTitle,
        };
        this.$emit('add-card', newCard);
        this.newCardTitle = '';
      }
    },
  },
  components: {
    ProductCard,
  },
};
</script>

<style scoped>
.column {
  padding: 1rem;
  flex: 1;
}

.column-title {
  margin-bottom: 1rem;
  font-size: 1.2rem;
  font-weight: bold;
}

.card-list {
  min-height: 200px;
  margin-bottom: 1rem;
}

.add-card {
  display: flex;
  align-items: center;
}

.card-input {
  flex: 1;
  padding: 0.5rem;
  margin-right: 0.5rem;
}

.add-button {
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 0.5rem 1rem;
  font-size: 1rem;
  cursor: pointer;
}
</style>
