<template>
  <section id="services">
    <div class="page-shell">
      <div class="split-grid align-start">
        <div class="stack">
          <span class="section-label">
            <span aria-hidden="true">::</span>
            What we do
          </span>
          <h2 class="section-title">Full-stack creative intelligence for modern GTM teams.</h2>
          <p class="body-lead">
            Our pods embed within your squads to craft experiments, design campaigns, and ship premium brand experiences. Every move is measured, reported, and iterated with growth compounding across your funnel.
          </p>
          <div class="tag-list">
            <span v-for="capability in capabilities" :key="capability">{{ capability }}</span>
          </div>
        </div>
        <div class="case-studies">
          <article v-for="service in services" :key="service.title" class="case-card">
            <header class="service-header">
              <span class="service-icon" aria-hidden="true">{{ service.icon }}</span>
              <div>
                <strong class="service-title">{{ service.title }}</strong>
                <span class="body-muted">{{ service.subtitle }}</span>
              </div>
            </header>
            <p class="body-muted">{{ service.description }}</p>
            <ul class="service-list">
              <li v-for="point in service.points" :key="point">
                <span aria-hidden="true">•</span>
                <span>{{ point }}</span>
              </li>
            </ul>
          </article>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { capabilities, services } from '~/constants/landing'
</script>
