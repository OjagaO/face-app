<template>
  <div>
    <video ref="video" width="640" height="480" autoplay></video>
    <canvas ref="canvas" width="640" height="480"></canvas>
  </div>
</template>

<script>
export default {
  mounted() {
    this.initCamera();
  },
  methods: {
    initCamera() {
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          this.$refs.video.srcObject = stream;
          this.trackFaces();
        })
        .catch((error) => console.error('Error accessing webcam:', error));
    },
    trackFaces() {
      const video = this.$refs.video;
      const canvas = this.$refs.canvas;
      const context = canvas.getContext('2d');

      tracking.Camera.init({
        video: video,
        onFrame: () => {
          context.clearRect(0, 0, canvas.width, canvas.height);
          tracking.track('#video', tracker);
        },
      });

      const tracker = new tracking.ObjectTracker(['face', 'eye']);
      tracker.setInitialScale(4);
      tracker.setStepSize(2);
      tracker.setEdgesDensity(0.1);

      tracker.on('track', (event) => {
        context.strokeStyle = '#a64ceb';
        context.lineWidth = 2;

        event.data.forEach((rect) => {
          context.beginPath();
          context.rect(rect.x, rect.y, rect.width, rect.height);
          context.stroke();

          // 顔にくっつけるメガネの画像を描画
          const glassesImage = new Image();
          glassesImage.src = 'https://svgsilh.com/svg_v2/311831.svg'; // メガネの画像へのパス
          const eyeWidth = rect.width / 5; // 眼の幅を基準にメガネの幅を調整
          const eyeHeight = rect.height / 5; // 眼の高さを基準にメガネの高さを調整
          const eyeX = rect.x + rect.width / 2 - eyeWidth / 2;
          const eyeY = rect.y + rect.height / 3 - eyeHeight / 2;
          context.drawImage(glassesImage, eyeX, eyeY, eyeWidth, eyeHeight);
        });
      });
    },
  },
};
</script>

<style>
/* 必要に応じてスタイリングを追加 */
</style>
