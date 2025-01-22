<template>
  <div>
    <div>Model: {{ model }}</div>
    <div>Manufacturer: {{ manufacturer }}</div>
    <table>
      <tbody>
        <tr v-for="(value, key) in deviceInfo" :key="`key-${value}`">
          <th style="text-align: right">{{ key }}</th>
          <td>{{ value }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { Device, type DeviceInfo } from '@capacitor/device'

const model = ref('Please wait...')
const manufacturer = ref('Please wait...')

const deviceInfo = ref<DeviceInfo>()

onMounted(() => {
  Device.getInfo().then((info) => {
    deviceInfo.value = info
    model.value = info.model
    manufacturer.value = info.manufacturer
  })
})
</script>
