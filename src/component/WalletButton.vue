<script setup lang="ts">
import { computed, ref } from 'vue'
import Modal from './Modal.vue'

const paidAmount = ref<number | null>(null)
const showModal = ref(false)

const displayAmount = computed(() => {
  if (!paidAmount.value) return 'پرداختی انجام نشده'
  return paidAmount.value.toLocaleString('fa-IR') + ' تومان'
})

const openModal = () => {
  showModal.value = true
}

const onPaid = (amount: number) => {
  paidAmount.value = amount
  showModal.value = false
}
</script>

<template>
  <div class="flex justify-center mt-10">
   
    <div
      @click="openModal"
      role="button"
      tabindex="0"
      @keydown.enter.prevent="openModal"
      @keydown.space.prevent="openModal"
      class="group flex w-fit items-center justify-between gap-3 rounded-xl border px-5 py-3 bg-white shadow-sm cursor-pointer border-gray-300 hover:border-blue-400 hover:bg-blue-50 focus-within:border-blue-500 focus-within:ring-1 focus-within:ring-blue-500 focus-visible:outline-none active:scale-[0.98] transition-all duration-300 ease-in-out"
      aria-label="افزایش اعتبار کیف پول"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="w-6 h-6 text-blue-500 group-hover:text-blue-400 transition-colors duration-300"
        viewBox="0 0 24 24"
        fill="currentColor"
        aria-hidden="true"
      >
        <path
          d="M17.48 12.2c-.5.48-.74 1.22-.54 1.98.25.93 1.17 1.52 2.13 1.52H20v1.45c0 2.21-1.79 4-4 4H6c-2.21 0-4-1.79-4-4v-7c0-2.21 1.79-4 4-4h10c2.2 0 4 1.8 4 4v1.45h-1.08c-.56 0-1.07.22-1.44.6Z"
        />
      </svg>

      <div
        class="flex gap-1 text-sm font-semibold text-gray-700 group-hover:text-blue-700 transition-colors duration-300"
        aria-live="polite"
      >
        <span>{{ displayAmount }}</span>
      </div>

      <div
        class="flex w-6 h-6 items-center justify-center rounded bg-blue-500 text-white text-lg font-bold transition-all duration-300 group-hover:bg-white group-hover:text-blue-500"
        aria-hidden="true"
      >
        +
      </div>
    </div>

    <Modal v-model="showModal" @paid="onPaid"  />
  </div>
</template>
