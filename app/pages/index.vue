<template>
  <div>
    <HeroSection />
    <TrustedSection />
    <ServicesSection />
    <CaseStudiesSection />
    <ProcessSection />
    <TestimonialsSection />
    <ContactSection />
  </div>
</template>

<script setup lang="ts">
import HeroSection from '~/components/sections/HeroSection.vue'
import TrustedSection from '~/components/sections/TrustedSection.vue'
import ServicesSection from '~/components/sections/ServicesSection.vue'
import CaseStudiesSection from '~/components/sections/CaseStudiesSection.vue'
import ProcessSection from '~/components/sections/ProcessSection.vue'
import TestimonialsSection from '~/components/sections/TestimonialsSection.vue'
import ContactSection from '~/components/sections/ContactSection.vue'

useSeoMeta({
  title: 'N² Marketing Studio',
  description: 'Marketing engineering studio delivering performance creative, narrative design, and analytics pods for ambitious product teams.',
  ogTitle: 'N² Marketing Studio — Creative systems for compounding growth',
  ogDescription: 'Full-stack marketing pods that connect brand, demand, and product marketing into one feedback loop.',
  twitterCard: 'summary_large_image'
})
</script>
