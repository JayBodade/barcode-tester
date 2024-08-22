<template>
    <div>
      <!--Giving Wrong Code  -->
      <div id="scanner-container" style="position: relative; width: 80%; height: 200px;"></div>
      <p v-if="barcode">Detected Barcode: {{ barcode }}</p>
      <button @click="startScanner">Start Scanner</button>
    </div>
  </template>
  
  <script>
  import Quagga from 'quagga';
  
  export default {
    data() {
      return {
        barcode: null,
      };
    },
    methods: {
      startScanner() {
        Quagga.init({
          inputStream: {
            type: 'LiveStream',
            target: document.querySelector('#scanner-container'),
            constraints: {
              facingMode: 'environment',
            },
          },
          decoder: {
            readers: [
              'code_128_reader',
              'ean_reader',
              'ean_8_reader',
              'code_39_reader',
              'code_39_vin_reader',
              'codabar_reader',
              'upc_reader',
              'upc_e_reader',
              'i2of5_reader',
              '2of5_reader',
              'code_93_reader',
            ],
          },
        }, (err) => {
          if (err) {
            console.error(err);
            return;
          }
          Quagga.start();
        });
  
        Quagga.onDetected(this.onDetected);
      },
      onDetected(result) {
        this.barcode = result.codeResult;
        
        Quagga.stop();
        console.log(`Detected barcode: ${this.barcode}`);
      },
    },
   
  };
  </script>
  
  <style>
  #scanner-container {
    width: 100%;
    max-width: 500px;
    height: 500px;
    margin: auto;
    position: relative;
  }
  </style>
  