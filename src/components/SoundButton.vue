<template>
  <label>
    <button class=" sound-btn btn"
    @click="playSound"
    v-bind:id = "keyTrigger[0] + keyTrigger[1]">
      <span> {{ keyTrigger[0] }} </span>
    </button>
  </label>
</template>

<script>
export default {
  name: 'SoundButton',
  props: ['keyTrigger', 'soundData', 'volume'],
  data: function() {
    return {
      audio: null
    };
  },
  mounted: function() {
    this.audio = new Audio(this.soundData.url);
    this.changeVolume();
  },
  watch: {
    volume: function() {
      this.changeVolume();
    }
  },
  methods: {
    playSound: function() {
      this.audio.play();
      this.$emit('message', `Played: ${this.soundData.id}`);
    },
    changeVolume: function() {
      this.audio.volume = this.volume/100;
    }
  }
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
