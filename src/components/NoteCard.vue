<script>
import popup from './popup.vue'
import updateNoteCard from './updateNoteCard.vue'

export default {
  data() {
    return {
      isVisiblePopup: false,
      isEditMenuOpen: false,
    }
  },
  components: {
    popup,
    updateNoteCard
  },
  props: ['product'],
  methods: {
    showPopup() {
      this.isVisiblePopup = true;
    },
    closePopup() {
      this.isVisiblePopup = false;
    },
    confirmPopup() {
      this.$emit('confirmPopup')
    },
  },
}
</script>

<template>
  <div class="card">
    <updateNoteCard :product="product" v-if="isEditMenuOpen" v-on:close-edit-menu="isEditMenuOpen = !isEditMenuOpen" />
    <popup :action="'Remove'" v-if="isVisiblePopup" v-on:remove-card="$emit('remove-card', product.id)"
      @close-remove-card-window="closePopup">
      {{ product.title }}
    </popup>
    <h3 class="card__title">{{ product.title }}</h3>
    <ul class="card__list">
      <li class="card__list-item" :class="{ 'card__list-item-completed': note.completed }" v-for="note of product.notes.slice(0, 5)"
        :key="note.id">
        {{ note.text }}
      </li>
    </ul>
    <div class="card__footer">
      <button class="card__btn card__btn_edit" @click="isEditMenuOpen = !isEditMenuOpen">Edit</button>
      <button class="card__btn card__btn_rm" @click="showPopup">Remove</button>
    </div>
  </div>
</template>

<style lang="scss">
$blue: steelblue;
$red: red;
$background-color: #fdf7ef;

.card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;

  margin: 10px;

  width: 200px;
  height: 300px;
  padding: 10px;

  background: $background-color;
  box-shadow: 6px 6px 5px gray;
  border-radius: 6px;

  &__title {
    margin: 0;

    font-family: 'Seaweed Script', cursive;
    font-size: 25px;
    font-weight: bold;
    text-align: center;

    border-bottom: 4px solid $red;
  }

  &__list {
    padding-left: 0;
    list-style: none;
    overflow: auto;
  }

  &__list-item {
    border-bottom: 2px solid $blue;

    font-family: 'Seaweed Script', cursive;
    font-style: italic;
    font-size: 20px;
  }

  &__list-item-completed {
    text-decoration: line-through;
    color: grey;
  }

  &__footer {
    display: flex;
    justify-content: space-between;
  }

  &__btn {
    padding: 10px 20px;

    border-radius: 4px;

    font-size: 15px;
    color: $background-color;
    text-transform: uppercase;

    transition-duration: 0.4s;

    &:hover {
      background-color: $background-color;
      color: black;
    }

    &_edit {
      border: 2px solid $blue;
      background-color: $blue;
    }

    &_rm {
      border: 2px solid $red;
      background-color: $red;
    }
  }
}
</style>  