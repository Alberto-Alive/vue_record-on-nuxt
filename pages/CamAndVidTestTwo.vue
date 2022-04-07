<template>
  <v-container>
    <media id="lol" />
        <v-btn class="btn btn-danger play" color="red" @click="openCamera()" title="Play"><i data-feather="play-circle"></i></v-btn>
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
      video.srcObject = stream;
    });

    document.getElementById("lol").appendChild(video);
      }
    }
}
</script>
