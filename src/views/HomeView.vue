<script setup>
import { onMounted, ref } from 'vue'

let loading = ref(false)
let status = ref([])
let settings = ref([])

onMounted(async () => {
  loading.value=true
  const statusResponse = await fetch('http://192.168.1.100/status', {
  })
  status.value = await statusResponse.json()
  console.log(status.value)

  const settingsResponse = await fetch('http://192.168.1.100/settings', {
  })
  settings.value = await settingsResponse.json()
  console.log(settings.value)
  loading.value=false
})
</script>

<template>
  <div v-if="loading">
    <p>loading...</p>
  </div>
  <div v-else>
    <div v-if="status && settings">
      <main>
        <div v-if="status.wifi_sta">
          <p>Connectée sur : {{ status.wifi_sta.ssid  ? status.wifi_sta.ssid : 'loading...' }}</p>
          <p>IP : {{ status.wifi_sta.ip  ? status.wifi_sta.ip : 'loading...' }}</p>
        </div>
        <div v-if="status.update && !status.update.new_version">
          <p>A jour : OUI</p>
        </div>
        <div v-else>
          <p>A jour : NON</p>
        </div>
        <div v-if="status.cloud">
          <p>Connecté au cloud : {{ status.cloud.connected  ? status.cloud.connected : 'loading...' }}</p>
          <!--      <p>A jour : {{ status.wifi_sta.ip  ? status.wifi_sta.ip : 'loading...' }}</p>-->
        </div>
      </main>
    </div>
    <div>
      <p>no data</p>
    </div>
  </div>
</template>
