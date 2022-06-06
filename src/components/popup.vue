<template>
  <div class="popup">
    <div class="popup__title">
      {{ this.action }}
      <span class="popup__card-title">
        <slot></slot>
      </span>
      ?
    </div>
    <div class="popup__actions">
      <button class="popup__btn popup__btn-no" @click="$emit('close-remove-card-window')">No</button>
      <button class="popup__btn popup__btn-yes" @click="$emit('remove-card')">Yes</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['action']
}
</script>

<style lang="scss">
$windowWidth: 400px;
$windowHeight: 120px;

.popup {
  position: fixed;
  top: calc(50vh - $windowHeight / 2);
  right: calc(50vw - $windowWidth / 2);
  z-index: 1;
  width: $windowWidth;
  height: $windowHeight;
  padding: 15px;

  display: flex;
  flex-direction: column;
  justify-content: space-between;

  background-color: #fff;
  box-shadow: 0 0 17px 0 #e7e7e7;

  &__title {
    font-family: 'Seaweed Script', cursive;
    font-style: italic;
    font-size: 20px;
  }

  &__card-title {
    font-weight: bold;
    font-size: 25px;
    font-style: normal;
    color: blue;
  }

  &__actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__btn {
    width: max-content;
    margin: 4px 2px;
    padding: 10px 32px;

    display: inline-block;

    font-size: 16px;
    color: white;
    text-align: center;
    text-decoration: none;

    border: none;

    cursor: pointer;
    transition-duration: 0.4s;

    &-yes {
      background-color: green;
    }

    &-no {
      background-color: red;
    }

    &:hover {
      box-shadow: 0 12px 16px 0 rgba(0, 0, 0, 0.24), 0 17px 50px 0 rgba(0, 0, 0, 0.19);
    }
  }
}
</style>