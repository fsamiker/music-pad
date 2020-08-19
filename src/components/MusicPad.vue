<template>
  <div class="pad">
    <div class='status-display'>
      <h2><p> {{ statusMessage }} </p></h2>
    </div>
    <div class="sounds" 
      v-if="soundOptions">
      <SoundButton
        v-for="item in soundMap"
        :key="item.sound.id"
        :buttonData="item"
        :volume="volume"
        @message="updateStatus"
        @record="recordSounds"
      ></SoundButton>
    </div>
    <div class="settings">
      <select v-model="currentSet" v-on:change="updateSoundSet">
        <option v-for="option in setOptions" v-bind:key="option.name" v-bind:value="option.value">
          {{ option.name }}
        </option>
      </select>
      <VolumeSlider v-model="volume" :volumeValue="volume"></VolumeSlider>
    </div>
    <div class="recorder">
      <div class="controls">
        <button @click="recording = !recording" class="record" :class="{'red': recording, 'black': !recording}">
          <i class="fas fa-circle"></i>
        </button>
        <button v-show="!play" @click="playRecording" :disabled="recordedColors.length == 0"
          @stop="play=!play" class="play">
          <i class="fas fa-play"></i>
        </button>
        <button v-show="play" @click="play=!play" class="stop">
          <i class="fas fa-stop"></i>
        </button>
        <button @click="clearRecording" class="clear" :disabled="recordedColors.length == 0">
          <i class="fas fa-minus-circle"></i>
        </button>
      </div>
      <div id="record-sequence" class="table">
        <ul>
          <li v-for="(s, i) in recordedColors" :key="i+s" :style="{'background-color': s}"></li>
        </ul>
      </div>
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
        colorOptions: ["#ff9f9f", "#ffaebe", "#f4c181", "#fbf690", "#a5f9e1", "#9ae397", "	#8fa8ec", "#ddb6ff", "#31374f"],
        currentSet: [0, 1, 2, 3, 4, 5, 6, 7, 8],
        volume: 50,
        statusMessage: 'SOUND PAD',
        recordedColors: [],
        recording: false,
        play:false,
        maxRecordLength: 50,
        recordedAudio: []
      };
  },
  computed: {
    soundMap: function() {
      let map = [];
      var i;
      for (i = 0; i < this.currentSet.length; i++) {
        map.push({
          'key': this.keyOptions[i], 
          'sound': this.soundOptions[this.currentSet[i]],
          'color': this.colorOptions[i]
          });
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
      recordSounds: function(color, audioUrl) {
        if (this.recordedColors.length > this.maxRecordLength) {
          this.updateStatus('Maximum Recording Length: ' + this.maxRecordLength);
        }
        else if (this.recording) {
          this.recordedColors.push(color);
          this.recordedAudio.push(audioUrl)
        }
      },
      playRecording: function() {
        this.play = !this.play;
        var audio = new Audio(this.recordedAudio[0]);
        var i = 1;

        audio.play();
        // Use arrow function perserves "this" object scope
        audio.onended = () => {
          if (i < this.recordedAudio.length & this.play==true) {
            audio.src = this.recordedAudio[i];
            audio.play();
            i++
          }
          else if (this.play == true) {
            this.play = !this.play;
          }
        }

      },
      clearRecording: function() {
        this.recordedAudio = [];
        this.recordedColors = [];
        this.updateStatus('Cleared Recording');
      }
  }
}
</script>

<style scoped>
.pad {
    display: grid;
    grid-template-columns: repeat(3, 1fr) 25%;
    grid-template-rows: 15% repeat(3, 1fr) 15%;
    height: clamp(600px, 80%, 750px);
    width: clamp(600px, 80%, 750px);
    background-color: #000000;
    background-image: linear-gradient(315deg, #4c4c4c 0%, #323232 65%);
    border-radius: 30px;
    padding: 10px;
    box-shadow: 0 5px 10px 0 black;
}

.status-display {
    grid-column: 1 / span 3;
    display: grid;
    place-items: center;
    padding-top: 20px;
}

.status-display h2 {
  font-family: 'Rajdhani', sans-serif;
  height: calc(100% - 20px);
  width: 70%;
  margin: 0;
  background-color: grey;
  color: black;
  border-style: solid;
  border-color: #b29100;
  justify-content: center;
  border-radius: 5px;
}

.sounds {
  gap: 10px;
  margin: 30px;
  grid-column: 1 / span 3;
  grid-row: 2 / span 3;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  place-items: center;
}

.settings {
  display: grid;
  grid-column: 4;
  grid-row: 2 / span 3;
  place-items: center;
  margin: 40% 20% 40% 0;

}

.recorder {
  grid-column: 1 / span 3;
  grid-row: 5;
  display: grid;
  place-items: center;
}

div#record-sequence {
  overflow: auto;
  width: 400px;
  white-space: nowrap;
  height: 60%;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

ul li{
  display: inline;
  color: black;
  padding: 2px;
  margin: 1px;
}

.recorder button {
  padding: 0;
  text-align: center;
  height: 30px;
  width: 30px;
  background-color: grey;
  border-radius: 10px;
  border-color: #b29100;
  margin: 0 10px 0 10px;
}

button.red {
  color: #8B0000;
}

button.black {
  color: black;
}

select {
  font-family: 'Rajdhani', sans-serif;
  font-weight: bold;
  font-size: 20px;
  color: black;
  border: 0;
  padding: 2px;
  background-color: grey;
}

</style>
