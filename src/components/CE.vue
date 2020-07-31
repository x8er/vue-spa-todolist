<template>
  <div class="CE">
    <!-- Edit name of note -->
    <input type="text" v-model="localNote.noteText" placeholder="Note name" />
    <div class="line"></div>
    <div class="create-task">
      <TodoAdd class="todo-add" />
      <!-- Input for adding a task -->
      <input
        type="text"
        class="todo-input"
        v-model="todoText"
        @keyup.enter="addTodoToList()"
        placeholder="Add task"
      />
    </div>
    <div class="todolist">
      <!-- Output of all tasks for editing -->
      <div v-for="(item, index) in localNote.todos" :key="index" class="todo">
        <input type="checkbox" :checked="item.checked" @click="item.checked = !item.checked" />
        <input type="text" v-model="item.data" />
        <!-- Deletes a task from todos -->
        <Trash class="trash" @click="localNote.todos.splice(index, 1)" />
      </div>
    </div>
    <!-- Sending data to the parent -->
    <div class="buttons">
      <Button type="save" @click.native="$emit('saveCE', localNote)" />
      <Button type="cancel" v-if="!note" @click.native="$emit('closeCE')" />
      <Button type="cancel" v-if="note" @click.native="popUpBG = true; popUpConfirmCancel = true" />
      <Button type="delete" v-if="note" @click.native="popUpBG = true; popUpConfirmDelete = true" />
    </div>
    <div class="popUpBG" v-if="popUpBG">
      <Confirm
        v-if="popUpConfirmCancel"
        q="Отменить изменение заметки?"
        @accept="popUpConfirmCancel = false; popUpBG = false; $emit('closeCE')"
        @cancel="popUpConfirmCancel = false; popUpBG = false"
      />
      <Confirm
        v-if="popUpConfirmDelete"
        q="Удалить заметку?"
        @accept="popUpConfirmDelete = false; popUpBG = false; $emit('deleteNote')"
        @cancel="popUpConfirmDelete = false; popUpBG = false"
      />
    </div>
  </div>
</template>

<script>
import Button from "../components/Button";
import Confirm from "../components/Confirm";
import TodoAdd from "../assets/todo-add.svg";
import Trash from "../assets/trash.svg";

export default {
  name: "CE",
  props: ["note"],
  data() {
    return {
      // Create local note
      localNote: {
        noteText: this.note ? this.note.noteText : "",
        todos: this.note ? this.note.todos.map((item) => ({ ...item })) : [],
      },
      todoText: "",
      popUpBG: false,
      popUpConfirmCancel: false,
      popUpConfirmDelete: false,
    };
  },
  methods: {
    // Adds a task to the local list after pressing enter
    addTodoToList() {
      this.localNote.todos.push({
        data: this.todoText,
        checked: false,
      });
      this.todoText = "";
    },
  },
  components: {
    Button,
    Confirm,
    TodoAdd,
    Trash,
  },
};
</script>

<style lang="scss" scoped>
.CE {
  position: relative;
  border-radius: 24px;
  border: 2px dashed #b3b3b3;
  padding: 20px;
  width: 100%;
  background-color: #f7f7f7;

  input {
    border-color: transparent;
    border-width: 0;
    border-bottom-width: 2px;
    border-style: solid;
    background-color: #f7f7f7;
    padding: 10px;
    font-size: 1.1rem;
    font-weight: 700;
    color: black;
    outline: none;
    transition: border-color 0.2s ease;
    width: 100%;

    &::placeholder {
      color: #bfbfbf;
      transition: color 0.2s ease;
    }

    &:focus::placeholder {
      color: black;
    }

    &:focus,
    &:hover {
      border-color: #7d7d7d;
    }
  }

  .line {
    border: 2px dashed black;
    margin: 20px 0;
  }

  .create-task {
    display: flex;
    align-items: center;

    .todo-add {
      width: 32px;
      height: 32px;
    }

    .todo-input {
      font-size: 1rem;
      font-weight: 400;
    }
  }

  .todolist {
    margin-top: 20px;

    .todo {
      display: flex;
      align-items: center;

      &:hover .trash {
        opacity: 1;
      }

      &:hover input {
        border-color: #7d7d7d;
      }

      input[type="checkbox"] {
        width: auto;
        margin: 0;
      }

      input {
        font-size: 1rem;
        font-weight: 400;
        padding: 2px 10px;
      }

      .trash {
        opacity: 0;
        border-radius: 16px;
        padding: 4px;
        width: 32px;
        height: 32px;
        fill: red;
        transition: all 0.2s ease;

        &:hover {
          background-color: red;
          fill: white;
        }
      }
    }
  }

  .buttons {
    display: flex;
    justify-content: flex-end;
    flex-wrap: wrap;
    margin-top: 20px;

    & > div {
      margin-left: 10px;
    }
  }

  .popUpBG {
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    background-color: rgba($color: #000000, $alpha: 0.5);
  }
}
</style>