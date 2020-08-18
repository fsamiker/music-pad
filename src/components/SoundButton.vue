<template>
  <label>
    <button class=" sound-btn btn"
    @click="playSound"
    v-bind:id = "keyTrigger[0] + keyTrigger[1]">
      <span></span>
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
      this.$emit('record', this.keyTrigger[0], this.soundData.url);
    },
    changeVolume: function() {
      this.audio.volume = this.volume/100;
    }
  }
}
</script>

<style scoped>

label {
  height: 100%;
  width: 100%;
}

.btn {
  background-color: #dbdbd5;
  text-align: center;
  padding: 20px;
  height: 100%;
  width: 100%;
  border-radius: 15px;
  opacity: 0.8;
  box-shadow: 0 2px 3px 0;
}
</style>
