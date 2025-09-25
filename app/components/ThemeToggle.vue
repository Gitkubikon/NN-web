<template>
  <div class="theme-toggle" role="group" aria-label="Theme toggle">
    <button
      v-for="option in options"
      :key="option.value"
      type="button"
      :aria-pressed="colorMode.preference === option.value"
      @click="colorMode.preference = option.value"
    >
      <span aria-hidden="true">{{ option.icon }}</span>
      <span class="sr-only">{{ option.label }}</span>
    </button>
  </div>
</template>

<script setup lang="ts">
const colorMode = useColorMode()

const options = [
  { value: 'light', label: 'Activate light theme', icon: '☀' },
  { value: 'dark', label: 'Activate dark theme', icon: '🌙' },
  { value: 'system', label: 'Match system theme', icon: '◎' }
]
</script>

<style scoped>
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
</style>
