<template>
  <div id="app">
    <section class="header">
      <img src="./assets/LOGO.svg" alt />
      <div class="ulist">
        <div>NEWS</div>
        <div :class="{'active': isActive === 'music'}">MUSIC</div>
        <div>VIDEO</div>
        <div>ARTIST</div>
        <div>EVENTS</div>
        <div>CONTACT</div>
      </div>
      <div class="login">
        <div>MY LIST</div>
        <img src="./assets/icon_member.svg" alt />
      </div>
    </section>
    <section class="body">
      <div>
        <div>
          <img src="./assets/album_1@2x.png" class="body-image" />
          <div>Palette</div>
          <div>IU</div>
          <div>2017-Apr</div>
        </div>
        <div>
          <div>
            <div>Music Videos</div>
            <img src="./assets/album_musicvideo@2x.png" class="big-image" />
          </div>
        </div>
      </div>
      <div>
        <div class="list" v-for="(item, index) in musicArray" :key="index" @click="sendItem(item)">
          <div>{{index + 1}}</div>
          <div>{{item.text}}</div>
          <img :src="item.isCollect ? heartF : heartE" @click.stop="isCollection(item)">
        </div>
      </div>
    </section>
    <section class="album">
      <div>
        <div>New Albums</div>
        <div class="album-content">
          <div v-for="(item, index) in albumArray" :key="index">
          <img :src="item.image" alt="">
            <div>{{item.text}}</div>
          </div>
        </div>
      </div>
    </section>
    <section class="footer">
      <audio-player ref="audio" :sources="audio" :loop="loops"
      :info="info"
      @set-music="setMusic"
      @return-music="returnMusic"
      @shuffle="shuffle"
      @loops="getLoops($event)"></audio-player>
    </section>
  </div>
</template>

<script>
import heartEmpty from '@/assets/icon_heart1.svg';
import heartFill from '@/assets/icon_heart2.svg';
import album2 from '@/assets/album_2@2x.png';
import album3 from '@/assets/album_3@2x.png';
import album4 from '@/assets/album_4@2x.png';
import album5 from '@/assets/album_5@2x.png';
import album6 from '@/assets/album_6@2x.png';
import iu from '@/assets/IU (아이유) - 이 지금 (Dlwlrma) (MP3 Audio) [Palette].mp3';
import iu2 from '@/assets/IU (아이유) - 팔레트 (Palette) (Feat. G-DRAGON) (MP3 Audio) [Palette].mp3';
import iu3 from '@/assets/IU (아이유) - 이런 엔딩 (Ending Scene) (MP3 Audio) [Palette].mp3';
import iu4 from '@/assets/IU (아이유) - 사랑이 잘 (Can’t Love You Anymore) (With 오혁 Ohhyuk) (MP3 Audio) [Palette].mp3';
import iu5 from '@/assets/IU (아이유) - 잼잼 (Jam Jam) (MP3 Audio) [Palette].mp3';
import iu6 from '@/assets/IU (아이유) - Black Out (MP3 Audio) [Palette].mp3';
import iu7 from '@/assets/IU (아이유) - 마침표 (Full Stop) (MP3 Audio) [Palette].mp3';
import iu8 from '@/assets/IU (아이유) - 밤편지 (Through The Night) (MP3 Audio) [Palette].mp3';
import iu9 from '@/assets/IU (아이유) - 그렇게 사랑은 (Love Alone) (MP3 Audio) [Palette].mp3';
import iu10 from '@/assets/IU (아이유) - 이름에게 (Dear Name) (MP3 Audio) [Palette].mp3';

import AudioPlayer from '@/components/AudioPlayer.vue';

export default {
  data() {
    return {
      isActive: 'music',
      heartE: heartEmpty,
      heartF: heartFill,
      musicArray: [
        {
          text: 'dlwlrma',
          isCollect: false,
          music: iu,
        },
        {
          text: 'Palette (Feat. G-DRAGON)',
          isCollect: false,
          music: iu2,
        },
        {
          text: 'Ending scene',
          isCollect: false,
          music: iu3,
        },
        {
          text: 'Cant Love You Anymore (With OHHYUK)',
          isCollect: false,
          music: iu4,
        },
        {
          text: 'Jam Jam',
          isCollect: false,
          music: iu5,
        },
        {
          text: 'Black Out',
          isCollect: false,
          music: iu6,
        },
        {
          text: 'Full Stop',
          isCollect: false,
          music: iu7,
        },
        {
          text: 'Through the Night',
          isCollect: false,
          music: iu8,
        },
        {
          text: 'Love Alone',
          isCollect: false,
          music: iu9,
        },
        {
          text: 'Dear Name',
          isCollect: false,
          music: iu10,
        },
      ],
      albumArray: [
        {
          text: 'BBIBBI',
          image: album2,
        },
        {
          text: 'SMASH HITS 2',
          image: album3,
        },
        {
          text: 'SoulMate',
          image: album4,
        },
        {
          text: 'Flower Bookmark',
          image: album5,
        },
        {
          text: 'CHAT-SHIRE',
          image: album6,
        },
      ],
      audio: [],
      storage: [],
      loops: false,
      info: {},
    };
  },
  methods: {
    /* eslint-disable */
    isCollection(item) {
      item.isCollect = !item.isCollect;
    },
    setMusic() {
      this.$refs.audio.stop();
      let index = this.musicArray.map(d => {
        return d.music;
      }).indexOf(this.audio[0]);
      this.audio.splice(0, 1, this.musicArray[index + 1].music);
      this.info = this.musicArray[index + 1];
      setTimeout(() => {
        this.$refs.audio.play();        
      }, 1100);
    },
    returnMusic() {
      this.$refs.audio.stop();
      let index  = this.musicArray.map(d => {
        return d.music;
      }).indexOf(this.audio[0]);
      this.audio.splice(0, 1, this.musicArray[index - 1].music);
      this.info = this.musicArray[index - 1];
      setTimeout(() => {
        this.$refs.audio.play();        
      }, 2000);
    },
    getLoops(e) {
      this.loops = e;
    },
    sendItem(i) {
      this.$refs.audio.stop();
      this.audio.splice(0, 1, i.music);
      this.info = i;
      setTimeout(() => {
        this.$refs.audio.play();        
      }, 2000);
    },
    shuffle() {
      this.$refs.audio.stop();
      const random = Math.round(Math.random(10) * 10);
      this.audio.splice(0, 1, this.musicArray[random].music);
      this.info = this.musicArray[random];
      setTimeout(() => {
        this.$refs.audio.play();        
      }, 2000);
    },
  },
  created() {
    this.info = this.musicArray[0];
    this.audio.push(this.musicArray[0].music);
  },
  components: {
    AudioPlayer,
  },
};
</script>

<style lang="scss">
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
}
* {
  box-sizing: border-box;
}
#app {
  background-color: #fcfbf6;
  padding: 25px 69px 150px 77px;
  min-width: 1366px;
  .header {
    display: flex;
    align-items: center;
    padding-bottom: 32px;
    & > img {
      margin-right: 68px;
    }
    .ulist,
    .login {
      display: flex;
      align-items: center;
    }
    .ulist {
      font-size: 14px;
      font-weight: bold;
      color: #aeaeae;
      .active {
        color: #707070;
      }
      & > div {
        margin-right: 50px;
        cursor: pointer;
        &:hover {
          color: #707070;
        }
      }
      & > div:last-child {
        margin-right: 0;
      }
    }
    .login {
      font-size: 14px;
      font-weight: bold;
      color: #aeaeae;
      margin-left: auto;
      & > div:first-child {
        margin-right: 30px;
      }
    }
  }
  .body {
    text-align: center;
    margin-top: 47px;
    display: flex;
    justify-content: center;
    .list {
      cursor: pointer;
    }
    & > div:first-child {
      width: 32%;
      padding-right: 63px;
      & > div:first-child {
        font-weight: bold;
        & > div:nth-child(2) {
          color: #707070;
          font-size: 24px;
          margin-top: 20px;
          margin-bottom: 5px;
        }
        & > div:nth-child(3) {
          color: #aeaeae;
          font-size: 20px;
          margin-bottom: 10px;
        }
        & > div:nth-child(4) {
          color: #aeaeae;
          font-size: 16px;
        }
      }
      & > div:last-child {
        margin-top: 30px;
        display: flex;
        justify-content: center;
        & > div {
          & > div:nth-child(1) {
            color: #707070;
            font-size: 16px;
            font-weight: bold;
            text-align: left;
            margin-bottom: 30px;
            margin-left: 5px;
          }
        }
        .big-image {
          width: 313px;
        }
      }
      .body-image {
        width: 300px;
      }
    }
    & > div:last-child {
      min-width: 472px;
      & > div {
        display: flex;
        padding-bottom: 25px;
        border-bottom: solid 1px #AEAEAE;
        margin-bottom: 26px;
        padding-left: 23px;
        & > div:nth-child(1) {
          font-size: 14px;
          color: #AEAEAE;
          margin-right: 27px;
        }
        & > div:nth-child(2) {
          font-size: 14px;
          color: #707070;
          font-weight: bold;
        }
        & > img:nth-child(3) {
          margin-left: auto;
          cursor: pointer;
          &:active {
            transform: scale(.8);
          }
        }
      }
    }
  }
  .album {
    display: flex;
    justify-content: center;
    margin-top: 13px;
    & > div {
      & > div:first-child {
        font-size: 16px;
        font-weight: bold;
        color: #707070;
        margin-bottom: 30px;
      }
      .album-content {
        display: flex;
        justify-content: center;
        text-align: center;
        & > div {
          margin-right: 65px;
          & > img {
            width: 115px;
            margin-bottom: 12px;
            transition-duration: .4s;
            cursor: pointer;
            &:hover {
              transform: scale(1.2);
            }
          }
          & > div {
            font-weight: bold;
            font-size: 12px;
            color: #707070;
          }
        }
        & > div:last-child {
          margin-right: 0;
        }
      }
    }
  }
  .footer {
    width: 100%;
    height: 78px;
    background-image: url('./assets/player_background@2x.png');
    position: fixed;
    bottom: 0;
    left: 0;
    background-size: cover;
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 1366px
  }
}
</style>
