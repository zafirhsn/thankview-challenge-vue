<template>

  <main>
    <div class="container">
      <div class="row">
        <div class="video-data">
          <h1 class="header">ThankView Walkthrough</h1>
          <p class="total-length">Total: {{totalVideoLength}}</p>
          <!-- Use a v-for to render -->
          <div class="list" v-for="(video, index) of videoData" :key=index :id=index @click="playVideo(video, index)">
            <span class="index"> {{index}}. </span>
            <span class="title"> {{video.title}}</span>
            <span class="length"> {{video.minutes}}m {{video.seconds}}s </span>
          </div>

        </div>

        <div class="video-play" id="selected-video">

        </div>
      </div>
    </div>
  </main>
</template>

<script>
  import walkThroughs from '../assets/walkthroughs.json'

  export default {
    data() {
      return {
        videoData: []
      }
    },
    methods: {
      test(event) {
        console.log(event)
        console.log(event.target.currentSrc);

        let url = event.target.currentSrc


        /* 
        This is a mess of copy and paste from below! Had to finish this in 3 minutes!
        
        */
        for (let i = 0; i < this.videoData.length; i++) {
          if (this.videoData[i].url === url) {
            if (i === this.videoData.length - 1) {
              let currentVideo = document.getElementById("selected-video");

              let nextVideo = `
                <video controls width="100%" poster="${this.videoData[0].thumb}">
                  <source src="${this.videoData[0].url}" type="video/mp4">
                </video>`

              currentVideo.innerHTML = nextVideo;

              let lastActive = document.getElementsByClassName("list active")
              console.log(lastActive)
              if (lastActive.length) {
                lastActive[0].classList.remove("active");
              }
              // Add active class to selected video
              let selectedList = document.getElementById(`${0}`);
              selectedList.classList.add("active")
            } else {
              let currentVideo = document.getElementById("selected-video");

              let nextVideo = `
                <video controls width="100%" poster="${this.videoData[i+1].thumb}">
                  <source src="${this.videoData[i+1].url}" type="video/mp4">
                </video>`
              currentVideo.innerHTML = nextVideo;
              currentVideo.getElementsByTagName("video")[0].play();

              let lastActive = document.getElementsByClassName("list active")
              console.log(lastActive)
              if (lastActive.length) {
                lastActive[0].classList.remove("active");
              }
              // Add active class to selected video
              let selectedList = document.getElementById(`${i+1}`);
              selectedList.classList.add("active")
            }
          }
        }

      },
      playVideo(video, index) {
        console.log(video.url);

        /* 
          1. Use video URL and thumb to play the selected video
        */  

      // Remove last active class
      let lastActive = document.getElementsByClassName("list active")
      console.log(lastActive)
      if (lastActive.length) {
        lastActive[0].classList.remove("active");
      }

      // Add active class to selected video
      let selectedList = document.getElementById(`${index}`);
      selectedList.classList.add("active")


      // Get video container
      let currentVideo = document.getElementById("selected-video");

      let nextVideo = `
        <video controls width="100%" poster="${video.thumb}">
          <source src="${video.url}" type="video/mp4">
        </video>`

      if (nextVideo === currentVideo.innerHTML) {
        return;
      }
      currentVideo.innerHTML = nextVideo;
      console.log(currentVideo.classList);
      currentVideo.getElementsByTagName("video")[0].play();
      currentVideo.getElementsByTagName("video")[0].addEventListener('ended', (event)=> {
        this.test(event);
      })


      }

    },
    watch: {

    },
    computed: {
      totalVideoLength() {
        let minutes = 0;
        let seconds = 0;
        for (let video of this.videoData) {
          minutes += video.minutes;
          seconds += video.seconds;
        }
        console.log(minutes, "m");
        console.log(seconds);
        minutes += Math.floor(seconds / 60);
        seconds = seconds % 60;
        return `${minutes}m ${seconds}s`;
      }

    },
    beforeCreate() {

    },
    created() {
      /* 
        1. Use JSON file to add video data to data object for component
      */
      walkThroughs.forEach((video)=> {
        this.videoData.push(video);
      })


    },
    beforeMount() {

    },
    mounted() {

      /* 
        1. When the DOM is loaded, play the first video
      */
      let currentVideo = document.getElementById("selected-video");
      let url = this.videoData[0].url;
      let thumb = this.videoData[0].thumb;
      let firstVideo = `
          <video controls width="100%" poster="${thumb}">
            <source src="${url}" type="video/mp4">
          </video>`
    currentVideo.innerHTML = firstVideo


    }

  }
</script>

<style lang="scss" scoped>
  main {
    height: 100%;
  }

  .container {
    display: flex;
    margin: 20px;
    justify-content: center;

  }

  .row {
    display:flex;
    box-sizing: border-box;
    // padding: 20px;
    width: 900px;
    height: 500px;
    background-color: #c6fbff
  }

  .video-data {
    width: 50%;
    overflow-y:auto;
    background-color: white;
  }

  .video-play {
    width: 50%;
    display: flex;
    padding: 20px;
    justify-content: center;
    align-items: center;
    background-color: #edeef0; 
  }

  .header {
    font-size: 24px;
    margin-top: 0;
    margin-bottom: 20px;
    padding-left: 20px;
    padding-top: 20px;
  }

  .total-length {
    padding-left: 20px;
  }

  .body {
    font-size: 16px;
  }

  .selected {
    background-color: #d4e2fe;
  }

  .list {
    box-sizing: border-box;
    border-style: solid;
    border-color: #EDEFF0;
    border-left: none;
    border-right: none;
    border-bottom: none;
    padding-top: 20px;
    padding-bottom: 20px;
    padding-right: 20px;

    &.active {
      background-color: #d4e2fe;
    }
  }

  .index {
    padding-left: 20px;
  }

  .length {
    position: relative;
    float: right;
  }

  .total-length {
    display: inline-block;
    margin-top: 0px;
    margin-bottom: 20px;
  }

</style>