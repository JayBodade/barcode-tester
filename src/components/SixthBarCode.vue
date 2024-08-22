<template>
    <v-container  class="d-flex flex-column align-center">
      <v-card max-width="600" class="video-container">
        <video ref="video" class="video" autoplay></video>
        <div class="overlay-frame">
          <svg class="scan-frame" viewBox="0 0 512 512">
            <path d="M336 448h56a56 56 0 0056-56v-56M448 176v-56a56 56 0 00-56-56h-56M176 448h-56a56 56 0 01-56-56v-56M64 176v-56a56 56 0 0156-56h56" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="10" stroke="white"></path>
          </svg>
        </div>
        <v-row justify="center" class="zoom-controls">
            <v-icon color="gray" @click="zoomIn">mdi-magnify-plus-outline</v-icon>
            <v-icon color="gray" @click="zoomOut">mdi-magnify-minus-outline</v-icon>
        </v-row>
        <v-overlay
          v-model="overlay"
          class="align-center justify-center"
          contained
        >
          <v-btn
            color="success"
            @click="this.overlay=false; startScanner()"
          >
            Reset
          </v-btn>
        </v-overlay>
      </v-card>
     
      <v-card-text class="barcode-result" v-if="barcode">Barcode Result: {{ barcode }}</v-card-text>
     
    </v-container>
  </template>
  
  <script>
  import { BarcodeDetector } from 'barcode-detector';
  
  export default {
    data() {
      return {
        barcode: null,
        videoStream: null,
        liveStreamStart: false,
        scanning: false,
        zoomLevel: 1,
        overlay:false,
      };
    },
    methods: {
      async startScanner() {
        if (!('BarcodeDetector' in window)) {
          console.error('Barcode Detector is not supported by this browser.');
          return;
        }
  
        try {
          this.videoStream = await navigator.mediaDevices.getUserMedia({
            video: {
              facingMode: 'environment',
              width: { ideal: 1920 },
              height: { ideal: 1080 }
            }
          });
  
          this.$refs.video.srcObject = this.videoStream;
          this.liveStreamStart = true;
          this.$refs.video.play();
          this.scanning = true;
          this.detectBarcode();
        } catch (error) {
          console.error('Error accessing the camera: ', error);
        }
      },
      async detectBarcode() {
        if (!this.scanning) return;
  
        const barcodeDetector = new BarcodeDetector({
          formats: [
            'code_128', 'ean_13', 'qr_code', 'aztec', 'codabar', 'code_39',
            'data_matrix', 'databar', 'databar_expanded', 'ean_8', 'itf',
            'upc_e', 'upc_a', 'rm_qr_code'
          ]
        });
  
        try {
          const barcodes = await barcodeDetector.detect(this.$refs.video);
          if (barcodes.length > 0) {
            this.overlay = true;
            this.stopScanner();
            this.barcode = barcodes[0].rawValue;
            console.log(`Detected barcode: ${this.barcode}`);
          } else {
            requestAnimationFrame(this.detectBarcode);
          }
        } catch (error) {
          console.error('Barcode detection failed: ', error);
          requestAnimationFrame(this.detectBarcode);
        }
      },
      stopScanner() {
        this.liveStreamStart = false;
        if (this.videoStream) {
          const tracks = this.videoStream.getTracks();
          tracks.forEach(track => track.stop());
          this.videoStream = null;
        }
        this.scanning = false;
        this.barcode = null;
      },
      zoomIn() {
        this.zoomLevel += 0.1;
        this.updateZoom();
      },
      zoomOut() {
        if (this.zoomLevel > 0.1) {
          this.zoomLevel -= 0.1;
          this.updateZoom();
        }
      },
      updateZoom() {
        this.$refs.video.style.transform = `scale(${this.zoomLevel})`;
        this.$refs.video.style.objectPosition = 'center center';
      }
    },
    mounted() {
      this.startScanner();
    }
  };
  </script>
  
  <style scoped>
  .video-container {
    position: relative;
    width: 100%;
    max-width: 600px;
    aspect-ratio: 16/9;
    overflow: hidden;
    border-radius: 10px;
    border:2px solid black;
  }
  
  .video {
    width: 100%;
    height: 100%;
    object-fit: cover;
     object-position: center center;
  }
  
  .overlay-frame {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .zoom-controls {
    position: absolute;
    bottom: 20px;
    right: 20px;
    gap: 10px;
  }
  
  .scan-frame {
    width: 80%;
    height: 80%;
  }
  
  .barcode-result {
    margin-top: 10px;
    font-size: 18px;
    font-weight: 500;
  }

  @media screen and (max-width: 600px) {
  .video-container {
    width: 100%; /* Ensure it doesn't exceed the screen width */
    height: auto;
  }
}
  </style>
  