<template>
  <div class="tv-wrapper">
    <video id="video" src="./../../assets/images/videoplayback.mp4" controls="true"></video>
    <div class="canvas-video"></div>
    <div class="pult">
      <button id="tv-pult-off" v-on:click="tvOff">Выключить TV</button>
      <button id="tv-pult-on" v-on:click="tvOn">Включить TV</button>
    </div>
  </div>
</template>

<script>
import barImg from "../../assets/images/bar.png";
export default {
  name: "MyCanvas",
  methods:{
    tvOn(){
      const pult = document.querySelector("#tv-pult-on");
      let video = document.querySelector('#video');
      pult.addEventListener("click", () => {
        video.play();
      })
    },
    tvOff(){
      const pult = document.querySelector("#tv-pult-off");
      let video = document.querySelector('#video');
      pult.addEventListener("click", () => {
        video.pause();
      })
    }
  },
  mounted() {
    window.onload = function () {
      let timerID;
      const { createCanvas } = require('canvas')
      let video = document.querySelector('video');
      const canvasWrapper = document.querySelector(".canvas-video");
      const canvas = createCanvas(canvasWrapper.clientWidth, canvasWrapper.clientWidth/1.7)
      const ctx = canvas.getContext('2d')
      canvasWrapper.appendChild(canvas)
      const img = new Image();
      img.onload = function () {
        // ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(img, 0, 0, img.width, img.height, 0, 0, canvas.width, canvas.height);
        ctx.beginPath();
      }
      img.src = barImg;
      let videoContext = canvas.getContext('2d');
      videoContext.fillRect(0,0,canvas.width,canvas.height);
      video.addEventListener("play", () => {
        timerID = window.setInterval(() => {
          videoContext.drawImage(video, (canvas.width/1.9175),(canvas.width/1.9175)/5.3079 , (canvas.width/4), (canvas.height/4));
          ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        },30);
      })
      video.addEventListener("stop", () => {
        clearInterval(timerID);
        videoContext.fillRect((canvas.width/1.9175),(canvas.width/1.9175)/5.3079 , (canvas.width/4), (canvas.height/4));
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
      })
      video.addEventListener("pause", () => {
        clearInterval(timerID);
        videoContext.fillRect((canvas.width/1.9175),(canvas.width/1.9175)/5.3079 , (canvas.width/4), (canvas.height/4));
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
      })

      window.onresize = function () {
        const canvasWrapper = document.querySelector(".canvas-video");
        canvasWrapper.innerHTML = "";
        const canvas = createCanvas(canvasWrapper.clientWidth, canvasWrapper.clientWidth/1.7)
        const ctx = canvas.getContext('2d')
        canvasWrapper.appendChild(canvas)
        const img = new Image();
        img.onload = function () {
          // ctx.clearRect(0,0, canvas.width, canvas.height)
          ctx.drawImage(img, 0, 0, img.width, img.height, 0, 0, canvas.width, canvas.height);
          ctx.beginPath();
        }
        img.src = barImg;
        let videoContext = canvas.getContext('2d');
        videoContext.fillRect(0,0,canvas.width,canvas.height);
      }
    }
  }
}

</script>

<style scoped>
.tv-wrapper {
  position: relative;

  width: 100%;
}
  template{
    position: relative;
  }
 video {
   width: 100%;
   position: absolute;
   visibility: hidden;
   left: 0;
   z-index: 123;

 }
  canvas, .canvas-video {
    width: 100%;
    left: 0;
  }
  canvas, img {
    image-rendering: optimizeQuality;
    image-rendering: -moz-crisp-edges;
    image-rendering: -webkit-optimize-contrast;
    image-rendering: optimize-contrast;
    -ms-interpolation-mode: nearest-neighbor;
  }
  .pult {
    display: flex;
    justify-content: space-around;
    position: absolute;
    bottom: 20px;
    left: 0;
    width: 100%;
  }
  #tv-pult-off, #tv-pult-on {
    background: none;
    border:2px solid #ffbe00;
    color: #ffbe00;
    border-radius: 16px;
    padding: 10px 20px;
    opacity: .5;
  }
  #tv-pult-off:hover, #tv-pult-on:hover {
    background: #ffbe00;
    border:2px solid #ffbe00;
    color: #000000;
    border-radius: 16px;
    padding: 10px 20px;
    opacity: .5;
  }

</style>