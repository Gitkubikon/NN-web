<template>
  <section id="hero">
    <div class="page-shell">
      <div class="hero-grid">
        <div class="hero-copy">
          <span class="section-label">
            <span aria-hidden="true">//</span>
            Performance creative collective
          </span>
          <h1 class="display-title">
            Launch marketing that compounds.<br />
            N<span class="text-gradient">²</span> orchestrates growth stories.
          </h1>
          <p class="body-lead">
            We blend brand, demand, and product marketing into one feedback loop. From positioning to pixel-perfect launch campaigns, our teams use live data to continuously iterate and scale your growth narrative.
          </p>
          <div class="action-row">
            <a href="#contact" class="button-primary">
              Start a project
              <span aria-hidden="true">→</span>
            </a>
            <a href="#case-studies" class="button-secondary">View case studies</a>
          </div>
          <div class="hero-metrics metrics-grid">
            <div v-for="metric in heroMetrics" :key="metric.label" class="metrics-card">
              <strong>{{ metric.value }}</strong>
              <span class="body-muted">{{ metric.label }}</span>
            </div>
          </div>
        </div>
        <ClientOnly>
          <HeroScene />
        </ClientOnly>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { heroMetrics } from '~/constants/landing'
import HeroScene from '~/components/HeroScene.client.vue'
</script>
