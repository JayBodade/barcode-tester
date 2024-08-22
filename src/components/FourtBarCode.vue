<template>
    <div>
      <div id="reader" style="width:500px"></div>
      <p v-if="barcode">Detected Barcode: {{ barcode }}</p>
    </div>
  </template>
  
  <script>
  import { Html5Qrcode } from "html5-qrcode";
  
  export default {
    data() {
      return {
        barcode: null,
        html5QrCode: null,
      };
    },
    methods: {
      startScanner() {
        const qrCodeSuccessCallback = (decodedText, decodedResult) => {
          // Handle the decoded text or result here
          this.barcode = decodedText;
          console.log(`Decoded text: ${decodedText}`, decodedResult);
        };
  
      
        
        const config = { fps: 10, 
            qrbox: { width: 250, height: 250 },
             };
        this.html5QrCode = new Html5Qrcode("reader");
        this.html5QrCode.start(
          { facingMode: "environment" },
          config,
          qrCodeSuccessCallback
        ).catch(err => {
          console.error(`Unable to start scanning, error: ${err}`);
        });
      }
    },
    mounted() {
      this.startScanner();
    },
   
  };
  </script>
  
  <style>
  #reader {
    width: 100%;
    max-width: 500px;
    margin: auto;
  }
  </style>