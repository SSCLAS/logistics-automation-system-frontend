<template>
    <div class="text-center">
      <video id="webcam-video" autoplay style="transform: scaleX(-1); width: 600px; height: 480px;"></video>
      <canvas id="output-canvas"></canvas>
      <div class="d-inline-flex justify-center" style="width: 100%;">
        <v-switch v-model="isWebcamOn" label="웹캠">
          <template v-slot:prepend>
            <v-icon color="black">mdi-camera</v-icon>
          </template>
        </v-switch>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        stream: null,
        videoElement: null,
        isWebcamOn: true, // 웹캠 기본값은 false로 설정
      };
    },
    mounted() {
      // 페이지 로드 시 초기값에 따라 웹캠 상태 설정
      if (this.isWebcamOn) {
        this.turnOnWebcam();
      }
    },
    watch: {
      isWebcamOn(newValue) {
        if (newValue) {
          this.turnOnWebcam();
        } else {
          this.turnOffWebcam();
        }
      },
    },
    methods: {
      turnOnWebcam() {
        navigator.mediaDevices.getUserMedia({ video: true })
          .then(stream => {
            this.stream = stream;
            this.videoElement = document.getElementById('webcam-video');
            this.videoElement.srcObject = stream;
          })
          .catch(error => {
            console.error('웹캠 스트림을 가져오는 도중 에러 발생:', error);
          });
      },
      turnOffWebcam() {
        if (this.stream) {
          this.stream.getTracks().forEach(track => track.stop());
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* 필요한 스타일 추가 */
  </style>
  