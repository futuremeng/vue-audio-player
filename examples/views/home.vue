<template>
  <div>
    <div @click="play">click here to set audioList</div>
    <div class="name">
      {{ currentAudioName }}
    </div>
    <audio-player
      ref="audioPlayer"
      :audio-list="playList"
      :before-play="handleBeforePlay"
      :is-loop="isLoop"
      :is-auto-play-next="true"
      @play="handlePlay"
      @loop="
        (val) => {
          isLoop = val
        }
      "
      @info="
        () => {
          console.log('info')
        }
      "
      @list-end="
        () => {
          console.log('list-end')
        }
      "
    >
    </audio-player>
  </div>
</template>

<script>
// import AudioPlayer from '@liripeng/vue-audio-player'
import AudioPlayer from '../../packages/index.js'

export default {
  components: {
    AudioPlayer,
  },

  data() {
    return {
      currentAudioName: '',
      audioList: [
        {
          name: 'audio 1',
          url: 'http://music.163.com/song/media/outer/url?id=317151.mp3',
        },
        {
          name: 'audio 2',
          url: 'http://music.163.com/song/media/outer/url?id=281951.mp3',
        },
      ],
      playList: [],
      isLoop: false,
    }
  },

  methods: {
    // Use this function if you want to do something before you start playing
    handleBeforePlay(next) {
      console.log('handleBeforePlay')
      if (
        this.audioList.length > 0 &&
        this.$refs.audioPlayer.currentPlayIndex > -1
      ) {
        this.currentAudioName =
          this.audioList[this.$refs.audioPlayer.currentPlayIndex].name
      }
      next() // Start play
    },
    play() {
      this.playList = this.audioList.map((elm) => elm.url)
      console.log('playList', this.playList)
      this.$refs.audioPlayer.play()
    },
    handlePlay() {
      // when user click play button before data is setted
      console.log('handlePlay outside')
      if (this.playList.length === 0 && this.audioList.length > 0) {
        this.play()
      }
    },
  },
  mounted() {
    // this.playList = [
    //   'http://music.163.com/song/media/outer/url?id=317151.mp3',
    //   'http://music.163.com/song/media/outer/url?id=281951.mp3',
    // ]
  },
}
</script>

<style scoped>
.name {
  text-align: center;
  line-height: 80px;
}
</style>
