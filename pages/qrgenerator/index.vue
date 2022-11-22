
<template>
  <div class="bg-gray-50">
    <form>
      <input type="text" v-model="QRValue">
    </form>
    <qrcode-vue v-if="QRValue" :value="QRValue" size="300" level="H" />
  </div>
</template>

<script>
import QrcodeVue from 'qrcode.vue'
export default {
  data() {
    return {
      QRValue: null,
    }
  },
  components: {
    QrcodeVue
  }
}
</script>