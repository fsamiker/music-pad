<template>
  <label>
    <button class=" sound-btn btn"
    @click="playSound(this)"
    v-bind:id = "buttonData.key[0] + buttonData.key[1]"
    :style="cssStyle">
      <span></span>
    </button>
  </label>
</template>

<script>
export default {
  name: 'SoundButton',
  props: ['buttonData', 'volume'],
  data: function() {
    return {
      audio: null
    };
  },
  computed: {
    cssStyle: function() {
      return {
        '--active-color': this.buttonData.color
      }
    }
  },
  mounted: function() {
    this.audio = new Audio(this.buttonData.sound.url);
    this.audio.preload = "auto";
    this.changeVolume();
  },
  watch: {
    volume: function() {
      this.changeVolume();
    }
  },
  methods: {
    playSound: function(button) {
      console.log(button);
      this.audio.play();
      this.$emit('message', `Played: ${this.buttonData.sound.id}`);
      this.$emit('record', this.buttonData.color, this.buttonData.sound.url);
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
  cursor: pointer;
  outline: none;
  border-radius: 15px;
  border: none;
  opacity: 0.8;
  box-shadow: 0 5px 5px 0 rgba(153, 153, 153, 0.486);
  transition: 0s 0.25s;
}

.btn:active {
  box-shadow: 0 5px rgba(102, 102, 102, 0.014);
  transform: translateY(2px);
  background-color: var(--active-color);
  transition: 0s;
}
</style>
