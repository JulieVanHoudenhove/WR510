<script setup>
import { onMounted, ref } from 'vue'

let loading = ref(false)
let data = ref([])

onMounted(async () => {
  loading.value=true
  const dataResponse = await fetch('https://shelly-77-eu.shelly.cloud/device/status?id=4022d88e30e8&auth_key=MWNiMjY5dWlk404459961993DCA83AE44BC6E3A6F58906952E7BECA0A5B69DC375C964915ACBC0EA536A0639CB73', {
  })
  data.value = await dataResponse.json()
  console.log(data.value)

  loading.value=false
})

function toggle() {
  data.value.data.device_status.relays[0].ison = !data.value.data.device_status.relays[0].ison
  let param = data.value.data.device_status.relays[0].ison ? 'on' : 'off'

  let myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/x-www-form-urlencoded");

  fetch(`https://shelly-77-eu.shelly.cloud/device/relay/control?channel=0&turn=${param}&id=4022d88e30e8&auth_key=MWNiMjY5dWlk404459961993DCA83AE44BC6E3A6F58906952E7BECA0A5B69DC375C964915ACBC0EA536A0639CB73`, {
    method: 'POST',
    headers: myHeaders,
    redirect: 'follow'
  })
      .then(response => response.json())
      .catch(error => console.log('error', error));
}
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
        <div v-if="data.data.device_status.relays">
          <p>Allumée : {{ data.data.device_status.relays[0].ison }}</p>
        </div>
      </main>
    </div>
    <div v-else>
      <p>no data</p>
    </div>
    <div>
      <button @click="toggle">Toggle</button>
    </div>
  </div>
</template>
