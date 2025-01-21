<template>
  <div>
    <q-btn color="primary" label="Get Picture" @click="captureImage" />

    <img v-if="imageSrc" :src="imageSrc" style="display: block; max-width: 100%" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Camera, CameraResultType } from '@capacitor/camera'

const imageSrc = ref<string | undefined>('')

async function captureImage() {
  const image = await Camera.getPhoto({
    quality: 100,
    // editing 功能有點廢 crop區塊沒辦法移動
    // allowEditing: true,
    resultType: CameraResultType.Uri,
    promptLabelHeader: '好啦拍張照啦',
    promptLabelCancel: '鼻要',
    promptLabelPhoto: '相簿選一張',
    promptLabelPicture: '現場拍一張',
  })

  //结果将因resultType选项的值而异。
  //CameraResultType.Uri-从image.webPath获取结果
  //CameraResultType.Base64-从image.base64String获取结果
  //CameraResultType.DataUrl-从image.DataUrl获取结果
  imageSrc.value = image.webPath
}
</script>
