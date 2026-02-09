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
    v-show="visible"
    class="fixed inset-0 z-50 flex  justify-center bg-black/40 backdrop-blur-sm md:flex md:items-center md:justify-center items-end"
  >
    <div
      class="bg-white h-[598px] w-[375px] md:h-154 md:w-125.25 rounded-lg transition-transform duration-300 ease-out md:translate-y-0"
      :class="visible ? 'translate-y-0' : 'translate-y-full'"
    >
      <div class="ml-6.5 flex justify-between">
        <h2
          class="text-[16px] w-[146px] h-[27px] mt-8 md:mt-[40px] mr-[116px] md:mr-[180px] font-semibold"
        >
          افزایش اعتبار کیف پول
        </h2>

        <button
          @click="close"
          class="w-5 h-5 mt-6 md:w-8 md:h-8 md:mt-6 flex pt-[2.5px] md:pt-0.75 md:pr-[0.5px] items-center justify-center cursor-pointer rounded-full border border-[#0A1F4D] text-[#0A1F4D]"
        >
          ✖
        </button>
      </div>

      <div
        class="mt-[33px] md:mt-6 flex items-center justify-between mr-6 ml-6 md:mr-17.5 md:ml-17.5"
      >
        <span class="text-[13px] md:text-[13px] text-[#8B95B1] font-normal"> موجودی فعلی </span>

        <h2
          v-if="amount"
          class="text-[#EB5757] md:text-[#DC362E] gap-1 text-[15px] md:text-[13px] font-medium"
        >
          {{ amount.toLocaleString('fa-IR') }} تومان
        </h2>

        <h2
          v-else
          class="text-brown-170 flex h-full items-center gap-1 text-[15px] md:text-[13px] font-medium text-navy-50"
        >
          {{ parseInt('0').toLocaleString('fa-IR') }} تومان
        </h2>
      </div>

      <div class="bg-[#CCCDDF] w-[327px] mr-[24px] md:w-[361px] mt-5 md:mt-4 h-px md:mx-auto"></div>

      <Selected :amount="amount" @update:amount="onAmountUpdate" />
      <PayMent :amount="amount" @paid="onPaid" />
    </div>
  </div>
</template>
