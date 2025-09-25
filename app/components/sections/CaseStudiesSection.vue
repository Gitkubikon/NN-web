<template>
  <section id="case-studies">
    <div class="page-shell">
      <div class="split-grid align-start">
        <div class="stack">
          <span class="section-label">
            <span aria-hidden="true">++</span>
            Results
          </span>
          <h2 class="section-title">Stories that move markets.</h2>
          <p class="body-lead">
            From PLG darlings to enterprise relaunches, N² builds layered campaigns that align brand and performance. Here are a few we can share publicly.
          </p>
          <a href="#contact" class="button-secondary">See engagement model</a>
        </div>
        <div class="case-studies">
          <article v-for="item in caseStudies" :key="item.title" class="case-card">
            <span class="section-label case-category">{{ item.category }}</span>
            <h3 class="case-title">{{ item.title }}</h3>
            <p class="body-muted">{{ item.description }}</p>
            <div class="case-metrics">
              <div v-for="metric in item.metrics" :key="metric.label">
                <strong class="case-metric-value">{{ metric.value }}</strong>
                <span class="body-muted">{{ metric.label }}</span>
              </div>
            </div>
            <a :href="item.href" class="button-secondary">{{ item.cta }} →</a>
          </article>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { caseStudies } from '~/constants/landing'
</script>
