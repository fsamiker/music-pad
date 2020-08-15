<template>
  <div class="pad">
    <h1>EPIC MUSIC PAD</h1>
    <div class="sounds" 
      v-if="soundOptions">
      <SoundButton
      v-for="k in keyOptions"
      :key="k[1]"
      :keyTrigger="k"
      @play="playSound"
      ></SoundButton>
    </div>
  </div>
</template>

<script>
import SoundButton from './SoundButton'

export default {
  name: 'MusicPad',
  components: {
    SoundButton
  },
  data: function() {
      return {
        defaultSets: {
          'Drums 1':[0, 1, 2, 3, 4, 5, 6, 7, 8],
          'Drums 2': [9, 10, 11, 12, 13, 14, 15, 16, 17] },
        keyOptions: Object.entries(require('./../keys.json').keys),
        soundOptions: null,
        currentSet: null
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
      this.currentSet = this.defaultSets['Drums 1'];
  },
  mounted: function() {
    window.addEventListener('keydown', e => {
      this.playSound(e.key.toUpperCase());
    });
  },
  methods: {
      loadSoundOptions: function() {
          return require('./../sound.json').soundSamples;
      },
      updateSoundSet: function(option) {
        this.currentSet = this.defaultSets[option];
      },
      randomSet: function() {

      },
      playSound: function(key) {
        if (key) {
          let sound = this.soundMap[key].url;
          var audio = new Audio(sound);
          audio.play();
        }
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
</style>
