<template>
  <!-- Well, this is just a custom button -->
  <div :class="[type]">
    <AddIcon v-if="type === 'add'" class="icon" />
    <SaveIcon v-if="type === 'save'" class="icon" />
    <ChangeIcon v-if="type === 'change'" class="icon" />
    <CancelIcon v-if="type === 'cancel'" class="icon" />
    <DeleteIcon v-if="type === 'delete'" class="icon" />
    <span>
      {{ type === 'add' ? 'CREATE NOTE' : '' }}
      {{ type === 'save' ? 'SAVE' : '' }}
      {{ type === 'change' ? 'CHANGE' : '' }}
      {{ type === 'cancel' ? 'CANCEL' : '' }}
      {{ type === 'delete' ? 'DELETE' : '' }}
    </span>
  </div>
</template>

<script>
import AddIcon from "../assets/add.svg";
import SaveIcon from "../assets/checked.svg";
import ChangeIcon from "../assets/change.svg";
import CancelIcon from "../assets/cancel.svg";
import DeleteIcon from "../assets/delete.svg";

export default {
  name: "Button",
  props: ["type"],
  data() {
    return {};
  },
  components: {
    AddIcon,
    SaveIcon,
    ChangeIcon,
    CancelIcon,
    DeleteIcon,
  },
};
</script>

<style lang="scss" scoped>
.add,
.save,
.change,
.cancel,
.delete {
  cursor: pointer;
  display: flex;
  align-items: center;
  width: max-content;
  border-radius: 24px;
  transition: box-shadow 0.2s ease;
  overflow: hidden;

  span {
    margin-right: 14px;
    line-height: 16px;
    color: white;
    font-weight: 600;
  }
}

.add {
  background-color: #0dbfba;

  &:hover {
    box-shadow: 0px 0px 8px 0px rgba(13, 191, 186, 1);
  }
}

.save {
  background-color: #5fbf02;

  &:hover {
    box-shadow: 0px 0px 8px 0px rgba(95, 191, 2, 1);
  }
}

.change {
  background-color: #0071e2;

  &:hover {
    box-shadow: 0px 0px 8px 0px rgba(0, 113, 226, 1);
  }
}

.cancel {
  background-color: #9b0025;

  &:hover {
    box-shadow: 0px 0px 8px 0px rgba(155, 0, 37, 1);
  }
}

.delete {
  background-color: #d31c70;

  &:hover {
    box-shadow: 0px 0px 8px 0px rgba(211, 28, 112, 1);
  }
}

.icon {
  width: 48px;
  height: 48px;
}
</style>