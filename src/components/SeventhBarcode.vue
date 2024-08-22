<template>
<capture-photo facing-mode="environment" camera-resolution="320x240"></capture-photo>
</template>
<script>
import { CapturePhoto } from './../../node_modules/@georapbox/capture-photo-element/dist/capture-photo-defined';
export default{
    components:{
        CapturePhoto
    }
    
}
</script>