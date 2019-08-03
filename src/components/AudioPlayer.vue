<template>
  <div class="audio-player">
    <img src="../assets/播放器專輯小圖@2x.png" class="img">
    <div>
      <div>{{info.text}}</div>
      <div>IU</div>
    </div>
    <div>
      <img src="../assets/icon_lastsong.svg" @click="returnMusic">
      <img :src="playing ? pauseImage : playImage" @click="togglePlayback()">
      <img src="../assets/icon_nextsong.svg" @click="setMusic">
    </div>
    <div>
      <div>{{seeks}}</div>
      <input type="range" class="volume" :value="progress * 100"
      @change="getProg($event);setProgress(progresss)"
      @mouseup="getProg($event);setProgress(progresss)"
      >
      <div>{{durations}}</div>
    </div>
    <div>
      <img src="../assets/icon_Progressiveplay.svg" @click="loops">
      <img src="../assets/icon_shuffle.svg" @click="shuffle">
      <img src="../assets/icon_volume.svg" @click="toggleMute()">
    </div>
    <div>
      <input type="range" class="volume" :value="volumes * 100"
      @change="getVol($event);setVolume(volumes)"
      @mouseup="getVol($event);setVolume(volumes)">
    </div>
  </div>
</template>

<script>
import VueHowler from 'vue-howler';
import pause from '@/assets/icon_pause.svg';
import play from '@/assets/icon_play.svg';

export default {
  mixins: [VueHowler],
  props: {
    info: {
      type: Object,
    },
  },
  data() {
    return {
      pauseImage: pause,
      playImage: play,
      volumes: 1,
      progresss: 0.01,
    };
  },
  methods: {
    getVol(e) {
      this.volumes = Number(e.target.value / 100);
    },
    getProg(e) {
      this.progresss = Number(e.target.value / 100);
    },
    setMusic() {
      this.$emit('set-music');
    },
    returnMusic() {
      this.$emit('return-music');
    },
    loops() {
      this.$emit('loops', true);
    },
    shuffle() {
      this.$emit('shuffle');
    },
  },
  computed: {
    seeks() {
      const min = Math.round(this.seek / 60) || '00';
      const sec = Math.round(this.seek % 60) < 10 ? `0${Math.round(this.seek % 60)}` : Math.round(this.seek % 60) || '00';
      return `${min}:${sec}`;
    },
    durations() {
      const min = Math.round(this.duration / 60);
      const sec = Math.round(this.duration % 60) < 10 ? `0${Math.round(this.duration % 60)}` : Math.round(this.duration % 60);
      return `${min}:${sec}`;
    },
  },
  watch: {
    loop(val) {
      this.loop = val;
    },
  },
};
</script>

<style lang="scss" scoped>
.audio-player {
  display: flex;
  align-items: center;
  width: 100%;
  justify-content: center;
  .img {
    width: 47px;
  }
  & > div:nth-child(2) {
    font-size: 14px;
    color: #707070;
    font-weight: bold;
    text-shadow: 0px 3px 6px rgba(0, 0, 0, .16);
    margin-left: 35px;
    & > div:first-child {
      margin-bottom: 5.79px;
      width: 144.7px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
  }
  & > div:nth-child(3) {
    display: flex;
    align-items: center;
    & > img {
      cursor: pointer;
    }
    & > img:nth-child(2) {
      margin: 0 25.33px 0 35.33px;
    }
  }
  & > div:nth-child(4) {
    display: flex;
    align-items: center;
    font-size: 14px;
    color: #707070;
    font-weight: bold;
    margin: 0 31.68px 0 35.55px;
    & > div:nth-child(1) {
      min-width: 35px;
    }
    & > div:nth-child(3) {
      min-width: 35px;
    }
    .volume {
      -webkit-appearance: none;
      overflow:hidden;
      width:450px;
      outline : none;
      position: relative;
      background: none;
      margin: 0 20px;
      &::-webkit-slider-thumb {
        -webkit-appearance: none;
        position: relative;
        width:3px;
        height:12px;
        background:#707070;
      }
    }
    .volume::before, .volume::after {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      width: 2000px;
      height: 3px;
      content:"";
      pointer-events: none;
      background: #707070;
    }
  }
  & > div:nth-child(5) {
    display: flex;
    align-items: center;
    & > img {
      margin-right: 20.84px;
      cursor: pointer;
    }
  }
  & > div:nth-child(6) {
    .progress {
      min-width: 113.97px;
      background-color: rgba(112, 112, 112, .4);
      height: 3px;
      margin: 0 20.14px 0 0;
      .progress-inner {
        background-color: #707070;
        height: 3px;
        position: relative;
        .square {
          position: absolute;
          right: 0;
          top: 70%;
          transform: translate(50%, -50%);
          height: 12px;
          width: 3px;
          background-color: #707070;
          cursor: pointer;
        }
      }
    }
    .volume {
      -webkit-appearance: none;
      overflow:hidden;
      width:113.97px;
      outline : none;
      position: relative;
      background: none;
      &::-webkit-slider-thumb {
        -webkit-appearance: none;
        position: relative;
        width:3px;
        height:12px;
        background:#707070;
      }
    }
    .volume::before, .volume::after {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      width: 2000px;
      height: 3px;
      content:"";
      pointer-events: none;
      background: #707070;
    }
  }
}
</style>
