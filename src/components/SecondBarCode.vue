<template>
    <!-- slower compared to first on -->
  <StreamQrcodeBarcodeReader @onloading="onLoading"
  @result="onResult"/>
  {{ result }}
</template>
<script>
import { StreamQrcodeBarcodeReader } from 'vue3-barcode-qrcode-reader';
export default{
    data(){
        return {
            result:null,
        }
    },
    components:{
        StreamQrcodeBarcodeReader
    },
    methods:{
        onResult(data){
            this.result = data;
        },
        onLoading(){
            this.result = 'loading';
        }
        
    }
    

}
</script>