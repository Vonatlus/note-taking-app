<script>
let id = 0;

import NoteCard from './components/NoteCard.vue'

export default {
  components: {
    NoteCard,
  },
  data() {
    return {
      products: [
        {
          id: id++,
          title: 'Purchases',
          notes: [
            { id: 123, text: 'Buy bread', completed: false },
            { id: 456, text: 'Buy cheese', completed: true },
            { id: 789, text: 'Buy onion', completed: false }
          ],
          isEditVisible: false,
        },
      ],
    }
  },
  methods: {
    createNewCard() {
      this.products.push({
        id: id++,
        title: 'Title',
        notes: [
        ],
        isEditVisible: false,
      })
    },
    removeCard(cardId) {
      this.products = this.products.filter(product => product.id !== cardId);
    },
  }
}
</script>

<template>
  <div class="App">
    <NoteCard v-for="product of products" :product="product" v-on:remove-card="removeCard" />
    <button class="create-new-card" @click="createNewCard">
      Create new card
    </button>

  </div>
</template>

<style lang="scss">
body {
  background-color: #f5f6fa;
}

.App {
  display: flex;
  flex-wrap: wrap;
  width: 100%;

}

.create-new-card {
  $green: green;

  margin: 10px;
  margin-top: calc(150px - 25px);

  height: 50px;
  padding: 10px 20px;

  border-radius: 4px;
  border: 2px solid $green;
  background-color: $green;

  font-size: 15px;
  color: white;
  text-transform: uppercase;

  transition-duration: 0.4s;

  &:hover {
    background-color: white;
    color: black;
  }
}
</style>
