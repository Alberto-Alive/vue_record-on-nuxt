<template>
  <v-container>
    <div class="display-cover">
    <video ref="video" autoplay></video>
    <canvas class="d-none"></canvas>

    <div class="video-options">
        <select name="" id="" class="custom-select">
            <option value="">Select camera</option>
        </select>
    </div>

    <img class="screenshot-image d-none" alt="">

    <div class="controls">
        <v-btn class="btn btn-danger play" color="red" @click="openCamera()" title="Play"><i data-feather="play-circle"></i></v-btn>
        <button class="btn btn-info pause d-none" title="Pause"><i data-feather="pause"></i></button>
        <button class="btn btn-outline-success screenshot d-none" title="ScreenShot"><i data-feather="image"></i></button>
    </div>
</div>
  </v-container>
</template>
<script>

export default {

  methods:{
    openCamera(){
    const video = document.createElement("video");
    video.setAttribute("playsinline", "");
    video.setAttribute("autoplay", "");
    video.setAttribute("muted", "");
    video.style.width = "200px";
    video.style.height = "200px";

    const facingMode = "user";
    const constraints = {
      audio: false,
      video: {
        facingMode
      }
    };

    navigator.mediaDevices.getUserMedia(constraints).then((stream) => {
      this.$refs.video.srcObject = stream;
    });

    document.body.appendChild(video);
      }
    }
}
</script>
