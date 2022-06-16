<template>
  <div class="listItem">
    {{ note.name }}
  </div>
</template>

<script>
export default {
  name: "ListItem",
  props: {
    note: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style scoped>
.listItem{
  text-align: center;
  font-family: Arial;
  font-size: 18px;
  padding: 10px;
  background-color: #c25205;
  border-radius: 15px;
  margin: 10px;
  cursor: pointer;
  transition: 0.15s linear
}
.listItem:hover{
  font-size: 22px;
}

</style>