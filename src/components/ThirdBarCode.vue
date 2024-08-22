<template>
    <div>
        <!-- not working -->
      <button @click="startScan">Start Scan</button>
      <div v-if="scanResult">
        <p>Scan Result: {{ scanResult }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import { BarcodeScanner } from '@capacitor-community/barcode-scanner';
  
  export default {
    data() {
      return {
        scanResult: null,
      };
    },
    methods: {
      async startScan() {
        try {
          // Check if user has granted camera permission
          const status = await BarcodeScanner.checkPermission({ force: true });
          if (!status.granted) {
            if (status.denied) {
              // camera permission was permanently denied
              alert('Please enable camera permission in settings');
            } else {
              // user can grant permission from here
              const permission = await BarcodeScanner.requestPermission();
              if (!permission.granted) {
                return;
              }
            }
          }
  
          // Hide background
          document.body.style.background = 'transparent';
          await BarcodeScanner.hideBackground(); // make background of WebView transparent
  
          const result = await BarcodeScanner.startScan(); // start scanning and wait for a result
  
          if (result.hasContent) {
            this.scanResult = result.content; // get the scanned content
          }
  
          // Reset background
          document.body.style.background = '';
          await BarcodeScanner.showBackground();
        } catch (error) {
          console.error('Barcode scan failed', error);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Add any specific styles for your component */
  </style>