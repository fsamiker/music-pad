<template>
  <label>
    <button class=" sound-btn btn"
    @click="$emit('play', keyTrigger[0])"
    v-bind:id = "keyTrigger[1]">
      <span> {{ keyTrigger[0] }} </span>
    </button>
  </label>
</template>

<script>
export default {
  name: 'SoundButton',
  props: ['keyTrigger'],
}
</script>

<style scoped>
.btn {
  background-color: red;
  text-align: center;
  padding: 20px;
  margin: 20px;
}
</style>
