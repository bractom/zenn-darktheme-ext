<script setup>
import { ref, watch } from 'vue';

const replaceCss = '/replace.css';

const isEnabled = ref(false);

watch(isEnabled, async () => {
  const [tab] = await chrome.tabs.query({ active: true, currentWindow: true });
  const param = {
    target: { tabId: tab.id || 0 },
    files: [ replaceCss ],
  };

  await chrome.scripting[isEnabled.value ? 'insertCSS' : 'removeCSS'](param);
});
</script>

<template>
  <h1>Zenn Dark Theme</h1>
  <button @click="isEnabled = !isEnabled">{{ isEnabled ? 'Disable' : 'Enable'}}</button>
</template>

<style scoped>
label {
  font-size: 20px;
}
</style>
