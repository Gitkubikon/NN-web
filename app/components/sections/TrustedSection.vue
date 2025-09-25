<template>
  <section aria-label="Trusted brands">
    <div class="page-shell">
      <div class="glass-panel trusted-panel">
        <p class="body-muted">Trusted by product-led teams scaling across five continents</p>
        <div class="trusted-logos" role="list">
          <span v-for="logo in logos" :key="logo" role="listitem">{{ logo }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { logos } from '~/constants/landing'
</script>
