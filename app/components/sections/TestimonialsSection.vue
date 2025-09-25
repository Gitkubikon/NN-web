<template>
  <section id="testimonials">
    <div class="page-shell">
      <div class="split-grid align-start">
        <div class="stack">
          <span class="section-label">
            <span aria-hidden="true">""</span>
            Partners speak
          </span>
          <h2 class="section-title">“N² is the growth partner you want in the war room.”</h2>
          <p class="body-lead">
            CMOs, founders, and product leaders trust us to interpret fast-moving data and translate it into creative market plays.
          </p>
        </div>
        <div class="testimonial-grid">
          <article v-for="quote in testimonials" :key="quote.name" class="case-card">
            <p class="testimonial-quote">“{{ quote.quote }}”</p>
            <div>
              <strong>{{ quote.name }}</strong>
              <span class="body-muted">{{ quote.role }} · {{ quote.company }}</span>
            </div>
          </article>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { testimonials } from '~/constants/landing'
</script>
