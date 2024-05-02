<template>
    <div>
      <input
        type="text"
        v-model="text"
        placeholder="Enter text to generate QR code"
      />
      <div ref="qrCodeContainer" class="qr-code-container"></div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted, watch } from 'vue';
  import QRCode from 'qrcode';
  
  export default {
    name: 'QRCodeGenerator',
    setup() {
      const text = ref('');
      const qrCodeContainer = ref(null);
      const canvasElement = ref(null);
  
      const generateQRCode = () => {
        if (canvasElement.value) {
          canvasElement.value.getContext('2d').clearRect(0, 0, canvasElement.value.width, canvasElement.value.height);
        }
  
        if (text.value.trim()) {
          QRCode.toCanvas(canvasElement.value, text.value, {
            width: 200,
            height: 200,
            colorDark: '#000000',
            colorLight: '#ffffff',
          });
        }
      };
  
      watch(text, generateQRCode);
  
      onMounted(() => {
        canvasElement.value = document.createElement('canvas');
        qrCodeContainer.value.appendChild(canvasElement.value);
        generateQRCode();
      });
  
      return {
        text,
        qrCodeContainer,
      };
    },
  };
  </script>
  
  <style scoped>
  .qr-code-container {
    margin-top: 10px;
  }
  </style>