<template>
    <!-- woking good only if there is big and clear barcode -->
  <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded"/>
  {{ result }}
</template>
<script>
import { StreamBarcodeReader } from 'vue-barcode-reader';
export default{
    data(){
        return {
            result:null,
        }
    },
    components:{
        StreamBarcodeReader
    },
    methods:{
        onDecode(result){
            this.result = result;
        },
        onLoaded(){
            this.result = 'Ready To Scan';
        }
    }

}
</script>