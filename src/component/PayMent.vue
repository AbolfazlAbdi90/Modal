<script setup lang="ts">
const props = defineProps<{ amount: number | null }>()

const emit = defineEmits<{
  (e: 'paid', amount: number): void
}>()

const pay = () => {
  if (!props.amount) {
    alert('لطفا مبلغ را انتخاب کنید')
    return
  }
  alert(`پرداخت به مبلغ ${props.amount.toLocaleString('fa-IR')} تومان انجام می‌شود.`)

  emit('paid', props.amount)
}
</script>

<template  >
  <h3 class="text-[14px] text-[#1A2F63]  font-medium mr-6 md:mr-[70px]">درگاه پرداخت</h3>

  <div
    class="bg-[#ECF1F9] w-[327px] md:w-[361px] h-32 mt-5 border border-gray-300 flex items-center mx-auto justify-center gap-[48px] md:gap-6 rounded-xl md:p-4"
  >
    <img
      class="bank-shadow border border-blue-500 object-contain cursor-pointer h-16 w-16 rounded-xl bg-white shadow-lg transition-transform duration-300 hover:scale-105 hover:border-blue-600"
      alt="بانک ۱"
      src="/image/Bank-Melat.png"
    />
    <img
      class="border border-gray-300 opacity-50 object-contain cursor-not-allowed h-16 w-16 rounded-xl bg-white shadow-md"
      alt="بانک ۲"
      src="/image/saman-bank.png"
    />
  </div>
  <div class="flex justify-center mt-[32px] md:mt-10">
    <button @click="pay" class="bg-[#1869EA] cursor-pointer text-[12px] md:text-[14px] w-[110px] h-[36px] md:w-45 md:h-11 text-white md:py-2 rounded-[7px]">افزایش موجودی</button>
  </div>
</template>
