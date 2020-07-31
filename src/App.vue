<template>
  <div id="app">
    <header>
      <p>
        <span>todo</span>
        <span class="ccc">list</span>
      </p>
      <!-- Button for create new note -->
      <Button type="add" @click.native="CEStatus = true" />
    </header>
    <!-- Using the component to interact with notes -->
    <CE v-if="CEStatus" @closeCE="CEStatus = false" @saveCE="addNote" />
    <div class="notelist">
      <!-- Cycle all notes -->
      <div class="note" v-for="(item, index) in notes" :key="index">
        <p class="title">{{ item.noteText }}</p>
        <div class="threetodos">
          <!-- Shows only first three todos in note -->
          <div class="todo" v-for="(todo, i) in item.todos.slice(0, 3)" :key="i">
            <input type="checkbox" :checked="todo.checked" disabled />
            <!-- Add class with text-decoration: line-through for checked todo -->
            <p :class="{ 'line-through': todo.checked }">{{ todo.data }}</p>
          </div>
        </div>
        <!-- Buttons for each note -->
        <div class="buttons">
          <Button type="change" @click.native="changeNote(index)" />
          <Button
            type="delete"
            @click.native="globalIndex = index; popUpConfirmRemoveNote = true; popUpBG = true;"
          />
        </div>
      </div>
    </div>
    <!-- Pop-ups -->
    <div v-show="popUpBG" class="popUpBG">
      <CE
        v-if="popUpCEStatus"
        :note="notes[globalIndex]"
        @saveCE="updateNote"
        @closeCE="cancelCE"
        @deleteNote="deleteNote(); closePopUpCE()"
      />
      <Confirm
        v-if="popUpConfirmRemoveNote"
        q="Вы хотите удалить эту заметку?"
        @accept="deleteNote(); closePopUpConfirm()"
        @cancel="popUpConfirmRemoveNote = false; popUpBG = false"
      />
    </div>
  </div>
</template>

<script>
import Button from "./components/Button";
import CE from "./components/CE";
import Confirm from "./components/Confirm";

export default {
  name: "App",
  data() {
    return {
      notes: [],
      CEStatus: false,
      popUpCEStatus: false,
      popUpBG: false,
      globalIndex: null,
      popUpConfirmRemoveNote: false,
    };
  },
  mounted() {
    this.notes = localStorage.notes ? JSON.parse(localStorage.notes) : [];
  },
  watch: {
    notes(newNotes) {
      localStorage.setItem("notes", JSON.stringify(newNotes));
    },
  },
  methods: {
    // Adds a note
    addNote(note) {
      this.notes.push(note);
      this.closeCE();
    },
    // Modifies the note
    changeNote(index) {
      this.globalIndex = index;
      this.openPopUpCE();
    },
    // Save changes to a modified note
    updateNote(note) {
      this.notes[this.globalIndex] = {
        noteText: note.noteText,
        todos: note.todos.map((item) => ({ ...item })),
      };
      this.globalIndex = null;
      this.closePopUpCE();
    },
    cancelCE() {
      this.globalIndex = null;
      this.closePopUpCE();
    },
    // Delete note
    deleteNote() {
      this.notes.splice(this.globalIndex, 1);
      this.globalIndex = null;
    },
    openPopUpCE() {
      this.popUpBG = true;
      this.popUpCEStatus = true;
    },
    closePopUpCE() {
      this.popUpCEStatus = false;
      this.popUpBG = false;
    },
    closePopUpConfirm() {
      this.popUpConfirmRemoveNote = false;
      this.popUpBG = false;
    },
    closeCE() {
      this.CEStatus = false;
    },
  },
  components: {
    Button,
    CE,
    Confirm,
  },
};
</script>

<style lang="scss">
#app {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  max-width: 51.13vw;
  width: 100%;
  position: relative;

  * {
    box-sizing: border-box;
  }

  header {
    padding: 20px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;

    p {
      position: relative;
      text-transform: uppercase;
      font-size: 1.5rem;
      line-height: 1.5rem;
      font-weight: 700;
      margin: 0;

      span {
        display: inline-block;
        line-height: 1.5rem;

        &.ccc {
          color: #0dbfba;
        }
      }
    }
  }

  .notelist {
    margin-top: 30px;

    .note {
      margin: 10px 0;
      padding: 20px;
      border-radius: 12px;
      border: 2px solid #b3b3b3;
      background-color: #f5f5f5;

      p {
        display: block;
        margin: 0;
        overflow-wrap: anywhere;

        &.line-through {
          text-decoration: line-through;
        }
      }

      .title {
        font-size: 1.1rem;
        font-weight: 700;
      }

      .threetodos {
        margin-top: 10px;

        .todo {
          display: flex;
          align-items: center;
        }
      }

      .buttons {
        display: flex;
        justify-content: flex-end;

        & > div {
          margin-left: 10px;
        }
      }
    }
  }

  > .popUpBG {
    position: fixed;
    top: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    padding: 0 24.43vw;
    background-color: rgba($color: #000000, $alpha: 0.5);
  }
}

@media screen and (max-width: 720px) {
  #app {
    max-width: 90vw;

    > .popUpBG {
      padding: 0 5vw;
    }
  }
}
</style>
