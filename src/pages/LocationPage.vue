<template>
  <div>
    <div>test: {{ test }}</div>
    <div>IS MOBILE {{ isNativePlatform }}</div>
    <div>GEO ID: {{ geoId }}</div>
    <div>
      Loading:
      <span v-if="position === null">Loading...</span>
      <span v-else>Loaded</span>
    </div>
    <div>permission {{ permission }}</div>
    <div>
      GPS position: <strong>{{ position }}</strong>
    </div>
    <button @click="requestPermissions">requestPermissions</button>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Geolocation, type Position } from '@capacitor/geolocation'
import { Capacitor } from '@capacitor/core'

const position = ref<Position | null>(null)

function getCurrentPosition() {
  Geolocation.getCurrentPosition().then((newPosition) => {
    console.log('Current', newPosition)
    position.value = newPosition
  })
}

let geoId: string = ''

const permission = ref()
const isNativePlatform = Capacitor.isNativePlatform()

const test = ref('')

const requestPermissions = async () => {
  try {
    permission.value = await Geolocation.requestPermissions()
  } catch (error) {
    test.value = error as string;
  }
}

onMounted(async () => {
  // if (isNativePlatform)
  // requestPermissions()

  permission.value = await Geolocation.checkPermissions()

  getCurrentPosition()
  // 我们开始监听
  geoId = await Geolocation.watchPosition({}, (newPosition) => {
    console.log('New GPS position')
    position.value = newPosition
  })
})

onBeforeUnmount(() => {
  // 我们做清理
  Geolocation.clearWatch({ id: geoId })
})
</script>
