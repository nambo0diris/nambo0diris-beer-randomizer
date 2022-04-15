<template>
  <div class="tv-wrapper">
    <video id="video" src="./../../assets/images/videoplayback.mp4" controls="true"></video>
    <canvas class="canvas-video"></canvas>
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
      let canvas = document.querySelector(".canvas-video");
      console.log(canvas)
      let video = document.querySelector('video');
      console.log(video)
      let ctx = canvas.getContext('2d');
      console.log(ctx)
      let videoContext = canvas.getContext('2d');
      videoContext.fillRect(0,0,canvas.width,canvas.height);
      ctx.imageSmoothingEnabled = true;
      ctx.webkitImageSmoothingEnabled = true;
      ctx.mozImageSmoothingEnabled = true;
      let image = new Image();
      image.onload = function () {
        // ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(image, 0, 0, image.width, image.height, 0, 0, canvas.width, canvas.height);
        ctx.beginPath();
      }
      image.src = barImg;
      video.addEventListener("play", () => {
        timerID = window.setInterval(() => {
          videoContext.drawImage(video, 155, 30.5, (canvas.width/4), (canvas.height/4)-7);
          ctx.drawImage(image, 0, 0, canvas.width, canvas.height);
        },30);
      })
      video.addEventListener("stop", () => {
        clearInterval(timerID);
        // videoContext.fillRect(155, 36, (canvas.width/4)-2, (canvas.height/4)-9);
        ctx.drawImage(image, 0, 0, canvas.width, canvas.height);
      })
      video.addEventListener("pause", () => {
        clearInterval(timerID);
        // videoContext.fillRect(155, 36, (canvas.width/4)-2, (canvas.height/4)-9);
        ctx.drawImage(image, 0, 0, canvas.width, canvas.height);
      })
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