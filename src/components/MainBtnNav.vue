<template>
  <button :class="{mainBtn:true, selected: selected}">{{ text }}</button>
</template>

<script>
export default {
  name: "MainBtnNav",
  props: {
    selected: Boolean,
    text:String
  }
}
</script>

<style scoped>

.mainBtn {
  height: 100%;
  background: none;
  border: none;
  color: #399c70;
  width: 170px;
  font-size: 18px;
  font-weight: 700;
  padding-top: 12px;
  outline: none;
  cursor: pointer;
  margin: 0;
}

.selected {
  border-bottom: 5px solid #399c70;
  height: 100%;
}

</style>