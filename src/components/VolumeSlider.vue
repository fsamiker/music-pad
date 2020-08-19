<template>
    <div class="slidecontainer">
      <button class="volume-btn" v-show="volumeValue > 0" @click="mute">
        <i class="fas fa-volume-up"></i>
      </button>
      <button v-show="volumeValue == 0" class="volume-btn" @click="mute">
        <i class="fas fa-volume-mute"></i>
      </button>
      <input type="range" min="0" max="100" class="slider" id="soundVolume"
          v-bind:value="volumeValue"
          v-on:input="$emit('input', $event.target.value)">
    </div>
</template>

<script>
export default {
  name: 'VolumeSlider',
  props: ['volumeValue'],
  data: function(){
    return {
      'previousVolume': 50
    }
  },
  methods: {
    mute: function() {
      if (this.volumeValue == 0) {
        this.$emit('input', this.previousVolume);
      }
      else{
        this.previousVolume = this.volumeValue;
        this.$emit('input', 0);
      }
    }
  }
}
</script>

<style scoped>
.slidecontainer{
  display: flexbox;
  white-space: nowrap;
  justify-content: space-around;
}

.slidecontainer input {
  width: minmax(25vw, 70%);
  margin: 10px;
}

.slidecontainer button {
  padding: 0;
  text-align: center;
  font-size: 0.5rem;
  height: 1.5rem;
  width: 1.5rem;
  background-color: grey;
  border-radius: 0.5rem;
  border-color: #b29100;
}

input[type="range"] {
 -webkit-appearance: none;
}

input[type="range"]:focus {
 outline: none;
}

input[type="range"]::-webkit-slider-runnable-track {
 background: #b29100;
 height: 5px;
}

input[type="range"]::-moz-range-track {
 background: #b29100;
 height: 5px;
}

input[type="range"]::-webkit-slider-thumb {
 -webkit-appearance: none;
 height: 15px;
 width: 15px;
 background: black;
 margin-top: -5px;
 border-radius: 50%;
}

input[type="range"]::-moz-range-thumb {
 height: 15px;
 width: 15px;
 background: black;
 margin-top: -5px;
 border-radius: 50%;
}

</style>