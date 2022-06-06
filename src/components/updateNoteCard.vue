<template>
  <div class="editWindow">
    <div class="editWindow__title">
      <input class="editWindow__title-input" type="text" placeholder="Enter title" v-model="copyProduct.title">
    </div>

    <form>
      <input type="text" class="editWindow__add-form" placeholder="Enter new note" v-model="newNote" />
      <button type="button" class="btn btn_add" @click="addNote">Add</button>
    </form>

    <ul class="list">
      <li class="list__item" v-for="note of copyProduct.notes" :key="note.id">
        <label class="list__label">
          <input class="list__checkbox" type="checkbox" v-model="note.completed" v-if="!note.isEditVisible">
          <span :class="{ 'note-title': note.completed }" v-if="!note.isEditVisible">{{ note.text }}</span>
          <input class="list__input" type="text" v-if="note.isEditVisible" v-model="note.text">
          <popup :action="'Remove'" v-if="isRemoveWindowOpen" @yes="removeNote(note.id)"
            @no="isRemoveWindowOpen = !isRemoveWindowOpen">
            {{ note.text }}
          </popup>
        </label>

        <div>
          <button class="btn btn_edit" @click="note.isEditVisible = !note.isEditVisible">✎</button>
          <button class="btn btn_delete" @click="isRemoveWindowOpen = !isRemoveWindowOpen">✘</button>
        </div>
      </li>
    </ul>

    <div class="editWindow__footer">
      <button class="btn btn_cancel" @click="isEditWindowOpen = !isEditWindowOpen">Cancel</button>
      <button class="btn btn_confirm" @click="edit">Confirm</button>
    </div>

    <popup :action="'Do you want leave edit menu'" v-if="isEditWindowOpen" @yes="$emit('close-edit-menu')"
      @no="isEditWindowOpen = !isEditWindowOpen">
      {{ copyProduct.title }}
    </popup>
  </div>
</template>

<script>
import popup from './popup.vue'

export default {
  props: ['product'],
  data() {
    return {
      newNote: '',
      isEditTitleOn: false,
      copyProduct: JSON.parse(JSON.stringify(this.product)),
      isRemoveWindowOpen: false,
      isEditWindowOpen: false,
    }
  },
  components: {
    popup
  },
  methods: {
    addNote() {
      if (this.newNote) {
        this.copyProduct.notes.push({
          id: new Date(), text: this.newNote, completed: false
        });
        this.newNote = '';
      }
    },
    removeNote(noteId) {
      this.copyProduct.notes = this.copyProduct.notes.filter(note => note.id !== noteId);
      this.isRemoveWindowOpen = !this.isRemoveWindowOpen;
    },
    edit() {
      this.product.title = this.copyProduct.title;
      this.product.notes = this.copyProduct.notes;
      this.$emit('close-edit-menu');
    },
  },
}
</script>

<style lang="scss">
$button-bg-color-add: #7f8ff4;
$button-bg-color-cancel: #f67b7b;
$button-bg-color-confirm: #67ac70;

$black: #363839;
$gray: #bdc1c6;
$white: #fff;
$green: #06842c;

.btn {
  display: inline-block;
  text-transform: uppercase;
  border: 0;
  outline: 0;
  transition: all 200ms ease-in;
  cursor: pointer;
  color: $white;
  box-shadow: 0 0 10px 2px rgba(0, 0, 0, .1);
  border-radius: 2px;
  padding: 12px 36px;

  &:active {
    box-shadow: inset 0 0 10px 2px rgba(0, 0, 0, .2);
  }

  &_add {
    background: $button-bg-color-add;
    margin-left: -66px;

    &:hover {
      background: darken($button-bg-color-add, 4%);
    }

    &:active {
      background: $button-bg-color-add;
    }
  }

  &_edit {
    background: $button-bg-color-add;
    padding: 5px 10px;
    margin: 3px;
  }

  &_delete {
    background: $button-bg-color-cancel;
    padding: 5px 10px;
    margin: 3px;
  }

  &_cancel {
    background: $button-bg-color-cancel;

    &:hover {
      background: darken($button-bg-color-cancel, 4%);
    }

    &:active {
      background: $button-bg-color-cancel;
    }
  }

  &_confirm {
    background: $button-bg-color-confirm;

    &:hover {
      background: darken($button-bg-color-confirm, 4%);
    }

    &:active {
      background: $button-bg-color-confirm;
    }
  }
}

.list {
  padding: 0;
  margin-bottom: 25px;

  max-height: 200px;
  overflow: auto;

  &__item {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__label {
    cursor: pointer;
    font-size: 1.5em;
    font-weight: 600;
    display: flex;
  }

  &__checkbox {
    position: relative;
    width: 1.5em;
    height: 1.5em;
    margin-right: 10px;
    color: $black;
    border: 1px solid $gray;
    border-radius: 4px;
    appearance: none;
    outline: 0;
    cursor: pointer;
    transition: background 175ms cubic-bezier(0.1, 0.1, 0.25, 1);

    &::before {
      position: absolute;
      content: '';
      display: block;
      bottom: 4px;
      left: 4px;
      width: 8px;
      height: 14px;
      border-style: solid;
      border-color: $white;
      border-width: 0 2px 2px 0;
      transform: rotate(45deg);
      opacity: 0;
    }

    &:checked {
      color: $white;
      border-color: $green;
      background: $green;

      &::before {
        opacity: 1;
      }
    }
  }

  &__input {
    border: 0;
    border-bottom: 2px solid #7f8ff4;
    width: 50%;
    font-size: 20px;
    font-weight: 600;
    text-align: center;
    color: #7f8ff4;

    &:focus {
      outline: 0;
      color: #7f8ff4;
    }

    &::placeholder {
      color: #BBDEFB;
    }
  }
}

.editWindow {
  position: fixed;
  top: 25vh;
  left: 30vw;

  padding: 20px;
  width: 400px;

  background-color: $white;
  box-shadow: 0 0 17px 0 #e7e7e7;

  &__title {
    position: relative;
    width: 100%;
    margin-bottom: 25px;
  }

  &__title-input {
    border: 0;
    border-bottom: 2px solid #7f8ff4;
    width: 95%;
    font-size: 30px;
    text-align: center;
    color: #7f8ff4;

    &:focus {
      outline: 0;
      color: #7f8ff4;
    }

    &::placeholder {
      color: #BBDEFB;
    }
  }

  &__add-form {
    width: 80%;
    margin-bottom: 25px;

    font-size: 20px;
    color: #7f8ff4;

    background: $white;
    box-shadow: 0 6px 10px 0 rgba(0, 0, 0, .1);
    border: 0;
    outline: 0;
    padding: 22px 18px;

    &::placeholder {
      color: #BBDEFB;
    }
  }

  &__footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
}

.note-title {
  text-decoration: line-through;
  color: $gray;
}
</style>