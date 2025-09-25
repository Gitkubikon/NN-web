<template>
  <section id="process">
    <div class="page-shell">
      <div class="split-grid align-start">
        <div class="stack">
          <span class="section-label">
            <span aria-hidden="true">&lt;&gt;</span>
            How we partner
          </span>
          <h2 class="section-title">Purpose-built pods for every growth moment.</h2>
          <p class="body-lead">
            We structure engagements around agile growth cycles. Each sprint ships a measurable outcome with strategic insight layered on top of execution.
          </p>
        </div>
        <div class="process-grid">
          <article v-for="step in process" :key="step.phase" class="process-card">
            <span class="process-phase">{{ step.phase }}</span>
            <strong class="process-title">{{ step.title }}</strong>
            <p class="body-muted">{{ step.description }}</p>
          </article>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { process } from '~/constants/landing'
</script>
