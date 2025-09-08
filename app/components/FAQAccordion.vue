<script setup lang="ts">
// Import JSON file
import faqItems from '~/assets/faq.json'

// Map JSON keys to Accordion format
const items = faqItems.map((faq, index) => ({
  label: faq.question,
  content: faq.answer,
  key: index
}))
</script>

<template>
  <div class="w-full max-w-4xl mx-auto">
    <UAccordion
      :items="items"
      variant="soft"
      color="blue"
      size="lg"
      class="rounded-2xl border border-white/10 bg-slate-900/60 backdrop-blur"
    />
  </div>
</template>
