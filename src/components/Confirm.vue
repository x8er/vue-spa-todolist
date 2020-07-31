<template>
  <div class="confirm">
    <div class="title">Подтверждение</div>
    <div class="question">{{ q }}</div>
    <div class="buttons">
      <div class="agree" @click="$emit('accept')">Accept</div>
      <div class="cancel" @click="$emit('cancel')">Cancel</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Confirm",
  props: ["q"],
};
</script>

<style lang="scss" scoped>
.confirm {
  border-radius: 12px;
  background-color: #f5f5f5;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  width: 300px;

  .title {
    color: white;
    padding: 10px 20px;
    font-weight: 700;
    background-color: red;
  }

  .question {
    padding: 10px;
    flex-grow: 1;
  }

  .buttons {
    display: flex;
    padding-bottom: 10px;
    justify-content: center;

    .agree,
    .cancel {
      cursor: pointer;
      display: flex;
      align-items: center;
      width: max-content;
      border-radius: 24px;
      padding: 10px 30px;
      margin: 0 10px;
      transition: box-shadow 0.2s ease;
      overflow: hidden;
      color: white;
      font-weight: 700;
      text-transform: uppercase;
    }

    .agree {
      background-color: #5fbf02;

      &:hover {
        box-shadow: 0px 0px 8px 0px rgba(95, 191, 2, 1);
      }
    }

    .cancel {
      background-color: #9b0025;

      &:hover {
        box-shadow: 0px 0px 8px 0px rgba(155, 0, 37, 1);
      }
    }
  }
}
</style>