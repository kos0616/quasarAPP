<template>
  <div>
    GPS position: <strong>{{ position }}</strong>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Geolocation } from '@capacitor/geolocation'

export default {
  setup() {
    const position = ref('determining...')

    function getCurrentPosition() {
      Geolocation.getCurrentPosition().then((newPosition) => {
        console.log('Current', newPosition)
        position.value = newPosition
      })
    }

    let geoId

    onMounted(() => {
      getCurrentPosition()

      // 我们开始监听
      geoId = Geolocation.watchPosition({}, (newPosition, err) => {
        console.log('New GPS position')
        position.value = newPosition
      })
    })

    onBeforeUnmount(() => {
      // 我们做清理
      Geolocation.clearWatch(geoId)
    })

    return {
      position,
    }
  },
}
</script>
