<template>
    <div class="slidecontainer">
    <input type="range" min="1" max="100" class="slider" id="soundVolume"
        v-bind:value="volumeValue"
        v-on:input="$emit('input', $event.target.value)">
    </div>
</template>

<script>
export default {
  name: 'VolumeSlider',
  props: ['volumeValue'],
}
</script>

<style scoped>
</style>