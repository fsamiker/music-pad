<template>
  <div class="pad">
    <h1>EPIC MUSIC PAD</h1>
    <div class="sounds" 
      v-if="soundOptions">
      <SoundButton
        v-for="k in keyOptions"
        :key="soundMap[k[0]].id"
        :keyTrigger="k"
        :soundData="soundMap[k[0]]"
        :volume="volume"
      ></SoundButton>
    </div>
    <div class="Settings">
      <VolumeSlider v-model="volume"></VolumeSlider>
      <select v-model="currentSet" v-on:change="updateSoundSet">
        <option v-for="option in setOptions" v-bind:key="option.name" v-bind:value="option.value">
          {{ option.name }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
import SoundButton from './SoundButton'
import VolumeSlider from './VolumeSlider'

export default {
  name: 'MusicPad',
  components: {
    SoundButton,
    VolumeSlider
  },
  data: function() {
      return {
        setOptions: [
          {'name': 'Heater Kit', 'value': [0, 1, 2, 3, 4, 5, 6, 7, 8]},
          {'name': 'Smooth Piano Kit', 'value': [9, 10, 11, 12, 13, 14, 15, 16, 17]}
        ],
        keyOptions: Object.entries(require('./../keys.json').keys),
        soundOptions: null,
        currentSet: [0, 1, 2, 3, 4, 5, 6, 7, 8],
        volume: 50
      };
  },
  computed: {
    soundMap: function() {
      let map = {};
      var i;
      for (i = 0; i < this.currentSet.length; i++) {
        map[this.keyOptions[i][0]] = this.soundOptions[this.currentSet[i]]
      }
      return map;
    }
  },
  created: function() {
      this.soundOptions = this.loadSoundOptions();
  },
  mounted: function() {
    window.addEventListener('keydown', e => {
      document.querySelector(`#${e.key}${e.keyCode}`.toUpperCase()).click();
    });
  },
  methods: {
      loadSoundOptions: function() {
          return require('./../sound.json').soundSamples;
      },
      updateSoundSet: function(e) {
        this.currentSet.splice(0, this.currentSet.length);
        this.currentSet.push(...e.target.value.split(","));
      }
  }
}
</script>

<style scoped>
.pad {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
    height: 400px;
    width: 500px;
    background-color: white;
}

h1 {
    grid-column: 1 / span 4;
}

.sounds {
  grid-column: 1 / span 3;
  grid-row: 2 / span 3;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  height: 100%;
  width: 100%;
}

.settings {
  grid-column: 4;
  grid-row: 2/ span 3;
}
</style>
