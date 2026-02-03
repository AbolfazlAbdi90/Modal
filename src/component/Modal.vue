<script setup lang="ts">
import { ref, computed } from 'vue'
import Selected from './Selected.vue'
import PayMent from './PayMent.vue'

const amount = ref<number | null>(null)

const props = defineProps<{
  modelValue: boolean
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: boolean): void
  (e: 'paid', amount: number): void
}>()

const visible = computed({
  get: () => props.modelValue,
  set: (val: boolean) => emit('update:modelValue', val),
})

const close = () => {
  visible.value = false
}

const onAmountUpdate = (newAmount: number | null) => {
  amount.value = newAmount
}

const onPaid = (paidAmount: number) => {
  emit('paid', paidAmount)
  visible.value = false
}
</script>

<template>
  <div
    v-if="visible"
    class="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm"
  >
    <div class="bg-white h-[634px] rounded-lg p-6 w-[546px]">
      <div class="flex justify-between">
        <h2 class="text-18px text-navy-10 font-semibold">افزایش اعتبار کیف پول</h2>
        <button
          class="ms-auto inline-flex size-8 items-center justify-center rounded-lg bg-transparent text-sm text-gray-400 transition-colors hover:bg-gray-200 hover:text-gray-900"
          @click="close"
        >
          ✖
        </button>
      </div>
      <div class="mt-4 flex items-center justify-between pr-10 pl-10">
        <span data-v-331d3d7d="" class="text-navy-70 text-13px ml-auto font-normal"
          >موجودی فعلی</span
        >
        <h2
          v-if="amount"
          class="text-brown-170 flex h-full items-center gap-1 font-medium text-navy-50"
        >
          {{ amount }} تومان
        </h2>
        <h2 class="text-brown-170 flex h-full items-center gap-1 font-medium text-navy-50" v-else>
          0 تومان
        </h2>
      </div>

      <Selected :amount="amount" @update:amount="onAmountUpdate" />

      <PayMent :amount="amount" @paid="onPaid" />
    </div>
  </div>
</template>
