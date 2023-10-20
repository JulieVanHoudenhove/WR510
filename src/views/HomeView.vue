<script setup>
import { onMounted, ref } from 'vue'

let loading = ref(false)
let data = ref([])

onMounted(async () => {
  loading.value=true
  const dataResponse = await fetch('https://shelly-86-eu.shelly.cloud/device/status?id=80646F827174&auth_key=MWRmYzM2dWlkE62C6C4C76F817CE0A3D2902F5B5D4C115E49B28CF8539114D9246505DE5D368D560D06020A92480', {
  })
  data.value = await dataResponse.json()
  console.log(data.value)

  loading.value=false
})
</script>

<template>
  <div v-if="loading">
    <p>loading...</p>
  </div>
  <div v-else>
    <div v-if="data.data && !loading">
      <main>
        <div v-if="data.data.device_status.wifi_sta">
          <p>Connectée sur : {{ data.data.device_status.wifi_sta.ssid }}</p>
          <p>IP : {{ data.data.device_status.wifi_sta.ip }}</p>
        </div>
        <div v-if="data.data.device_status.update && !data.data.device_status.update.new_version">
          <p>A jour : OUI</p>
        </div>
        <div v-else>
          <p>A jour : NON</p>
        </div>
        <div v-if="data.data.device_status.cloud">
          <p>Connecté au cloud : {{ data.data.device_status.cloud.connected }}</p>
        </div>
      </main>
    </div>
    <div v-else>
      <p>no data</p>
    </div>
  </div>
</template>
