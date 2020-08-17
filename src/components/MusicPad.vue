<template>
  <div class="pad">
    <div class='status-display'>
      <h2> {{ statusMessage }} </h2>
    </div>
    <div class="sounds" 
      v-if="soundOptions">
      <SoundButton
        v-for="k in keyOptions"
        :key="soundMap[k[0]].id"
        :keyTrigger="k"
        :soundData="soundMap[k[0]]"
        :volume="volume"
        @message="updateStatus"
        @record="recordSounds"
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
    <div class="recorder">
      <button @click="recording = !recording">Record</button>
      <button v-if="!play" @click="playRecording" :disabled="recordedSounds.length == 0">Play</button>
      <button v-if="play" @click="play=!play">Stop</button>
      <button @click="recordedSounds=[]">Clear</button>
      <ul id="record-sequence">
        <li v-for="(s, i) in recordedSounds" :key="i+s"> {{ s }} </li>
      </ul>
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
        volume: 50,
        statusMessage: 'SOUND PAD',
        recordedSounds: [],
        recording: false,
        play:false,
        maxRecordLength: 50,
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
  watch: {
    volume: function() {
      this.statusMessage = `Volume: ${this.volume}`;
    }
  },
  methods: {
      loadSoundOptions: function() {
          return require('./../sound.json').soundSamples;
      },
      updateSoundSet: function(e) {
        this.currentSet.splice(0, this.currentSet.length);
        this.currentSet.push(...e.target.value.split(","));
      },
      updateStatus: function(message) {
        this.statusMessage = message;
      },
      recordSounds: function(key) {
        if (this.recordedSounds.length > this.maxRecordLength) {
          this.updateStatus('Reached Maximum Recording Length: ' + this.maxRecordLength);
        }
        else if (this.recording) {
          this.recordedSounds.push(key);
        }
      },
      playRecording: function() {
        // Disable recording to prevent infinite loop
        const currentRecordState = this.recording;
        this.recording = false;
        this.play = !this.play;
        for (var i = 0; i < this.recordedSounds.length; i++) {
          if (!this.play) {
            break
          }
          document.querySelector(`#${this.recordedSounds[i]}`.toUpperCase()).click();
        }
        this.recording = currentRecordState;
        this.play = !this.play;
      }
  }
}
</script>

<style scoped>
.pad {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(5, 1fr);
    height: 400px;
    width: 500px;
    background-color: white;
}

.status-display {
    grid-column: 1 / span 4;
}

.sounds {
  grid-column: 1 / span 3;
  grid-row: 2 / span 3;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
}

.settings {
  grid-column: 4;
  grid-row: 2/ span 3;
}

.recorder {
  grid-column: 1 / span 4;
  grid-row: 5;
}

ul {
  overflow: auto;
  width: 400px;
  white-space: nowrap;
}

ul#record-sequence li{
  display: inline;
}
</style>
