<template>
  <div>
    <video part="video" playsinline></video>

<canvas hidden></canvas>

<div part="actions-container">
  <slot name="capture-button">
    <button part="capture-button" type="button">
      <slot name="capture-button-content">Capture photo</slot>
    </button>
  </slot>

  <slot name="facing-mode-button" hidden>
    <button part="facing-mode-button" type="button">
      <slot name="facing-mode-button-content">Toggle facing mode</slot>
    </button>
  </slot>

  <slot name="actions"></slot>
</div>

<slot></slot>

<div part="output-container" id="output"></div>
    
  </div>
</template>

<script>


export default {
  name: 'CapturePhotoComponent',
  components:{
    
  },
  methods: {
    handlePhotoCaptured(event) {
      const photo = event.detail.photo;
      console.log('Photo captured:', photo);
    },
    handleScanButtonClick() {
      this.$refs.capturePhotoElement.takePhoto();
    },
  },
};
</script>

<style scoped>
 :host {
    display: block;
    box-sizing: border-box;
  }

  :host *,
  :host *::before,
  :host *::after {
    box-sizing: inherit;
  }

  :host([hidden]),
  [hidden],
  ::slotted([hidden]) {
    display: none;
  }

  video {
    display: block;
  }

  #output:empty {
    display: none;
  }

</style>
