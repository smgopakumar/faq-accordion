<script setup lang="ts">
import items from "~/assets/faq.json";
</script>

<template>
  <div class="min-h-screen bg-slate-950 text-white">
    <div class="px-4 sm:px-6 lg:px-8 py-10">
      <h1 class="text-2xl sm:text-3xl font-semibold mb-6">FAQs</h1>
      <FaqAccordion :items="items" :singleOpen="true" />
    </div>
  </div>
</template>
