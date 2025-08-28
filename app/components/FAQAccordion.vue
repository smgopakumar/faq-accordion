<script setup lang="ts">
import { ref } from "vue";

type FaqItem = {
  question: string;
  answer: string;
};

const props = defineProps<{
  items: FaqItem[];
  singleOpen?: boolean;
}>();

const singleOpen = props.singleOpen ?? true;
const openIndex = ref<number | null>(null);
const openSet = ref<Set<number>>(new Set());

function toggle(i: number) {
  if (singleOpen) {
    openIndex.value = openIndex.value === i ? null : i;
  } else {
    if (openSet.value.has(i)) openSet.value.delete(i);
    else openSet.value.add(i);
  }
}

function isOpen(i: number) {
  return singleOpen ? openIndex.value === i : openSet.value.has(i);
}
</script>

<template>
  <div class="w-full max-w-4xl mx-auto">
    <div
      class="divide-y divide-white/10 rounded-2xl border border-white/10 bg-slate-900/60 backdrop-blur"
    >
      <div v-for="(item, i) in items" :key="i" class="group">
        <!-- Header button -->
        <button
          type="button"
          class="w-full flex items-center justify-between px-5 py-4 text-left focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500"
          :aria-expanded="isOpen(i)"
          :aria-controls="`faq-panel-${i}`"
          @click="toggle(i)"
        >
          <span class="text-base sm:text-lg font-medium text-slate-100">
            {{ item.question }}
          </span>

          <!-- Chevron -->
          <svg
            class="h-5 w-5 shrink-0 transition-transform duration-300"
            :class="isOpen(i) ? 'rotate-180' : 'rotate-0'"
            viewBox="0 0 20 20"
            fill="currentColor"
            aria-hidden="true"
          >
            <path
              fill-rule="evenodd"
              d="M5.23 7.21a.75.75 0 011.06.02L10 10.94l3.71-3.71a.75.75 0 111.06 1.06l-4.24 4.24a.75.75 0 01-1.06 0L5.21 8.29a.75.75 0 01.02-1.08z"
              clip-rule="evenodd"
            />
          </svg>
        </button>

        <!-- Content with smooth height transition -->
        <transition
          enter-active-class="transition-[max-height,opacity] duration-300 ease-out"
          leave-active-class="transition-[max-height,opacity] duration-300 ease-in"
          enter-from-class="max-h-0 opacity-0"
          enter-to-class="max-h-96 opacity-100"
          leave-from-class="max-h-96 opacity-100"
          leave-to-class="max-h-0 opacity-0"
        >
          <div
            v-show="isOpen(i)"
            :id="`faq-panel-${i}`"
            role="region"
            class="px-5 pb-5 text-slate-200/90"
          >
            <p class="text-sm sm:text-base leading-relaxed">
              {{ item.answer }}
            </p>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* ensure the transition doesn't jump */
[role="region"] {
  overflow: hidden;
}
</style>
