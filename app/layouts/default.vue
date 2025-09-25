<template>
  <div class="page-root">
    <NuxtRouteAnnouncer />

    <header class="site-header">
      <div class="page-shell">
        <nav class="site-nav" aria-label="Primary">
          <a class="logo-mark" href="#hero" aria-label="N squared home">
            <span>N</span>
            <sup>2</sup>
          </a>

          <button
            class="site-nav-toggle"
            type="button"
            aria-label="Toggle navigation"
            @click="isMobileNavOpen = !isMobileNavOpen"
          >
            <span />
            <span />
            <span />
          </button>

          <ul class="site-nav-list" :class="{ 'is-open': isMobileNavOpen }">
            <li v-for="link in navLinks" :key="link.href">
              <a :href="link.href" @click="isMobileNavOpen = false">{{ link.label }}</a>
            </li>
          </ul>

          <div class="site-nav-actions">
            <ThemeToggle />
            <a href="#contact" class="button-secondary">Book a call</a>
          </div>
        </nav>
      </div>
    </header>

    <main class="page-main">
      <slot />
    </main>

    <footer class="site-footer">
      <div class="page-shell">
        <div class="footer-grid">
          <div class="stack">
            <a class="logo-mark" href="#hero" aria-label="N squared home">
              <span>N</span><sup>2</sup>
            </a>
            <p class="body-muted">
              N² is a marketing engineering studio connecting storytelling, systems, and performance to unlock enduring demand.
            </p>
          </div>
          <div class="footer-links">
            <strong>Explore</strong>
            <a href="#services">Services</a>
            <a href="#case-studies">Case studies</a>
            <a href="#process">Process</a>
            <a href="#testimonials">Testimonials</a>
          </div>
          <div class="footer-links">
            <strong>Connect</strong>
            <a href="mailto:hello@n2.agency">hello@n2.agency</a>
            <a href="https://www.linkedin.com" target="_blank" rel="noopener">LinkedIn</a>
            <a href="https://x.com" target="_blank" rel="noopener">X / Twitter</a>
            <a href="https://dribbble.com" target="_blank" rel="noopener">Dribbble</a>
          </div>
          <div class="footer-links">
            <strong>Resources</strong>
            <a href="#" aria-disabled="true">Insights (coming soon)</a>
            <a href="#" aria-disabled="true">Community</a>
            <a href="#" aria-disabled="true">Brand toolkit</a>
          </div>
        </div>
        <p class="footer-meta">© {{ currentYear }} N² Studio. Crafted in Berlin &amp; San Francisco.</p>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { navLinks } from '~/constants/landing'
import { onBeforeUnmount, onMounted, ref } from 'vue'

const currentYear = new Date().getFullYear()
const isMobileNavOpen = ref(false)

const handleKeydown = (event: KeyboardEvent) => {
  if (event.key === 'Escape') {
    isMobileNavOpen.value = false
  }
}
onMounted(() => window.addEventListener('keydown', handleKeydown))
onBeforeUnmount(() => window.removeEventListener('keydown', handleKeydown))
</script>
