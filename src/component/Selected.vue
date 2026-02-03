<script setup lang="ts">
import { ref } from 'vue'

const presetAmounts = [1000000, 3000000, 5000000, 10000000]

const rawValue = ref('')
const formattedValue = ref('')
const amountInWords = ref('')
const selectedAmount = ref<number | null>(null)
const errorMessage = ref('')

const emit = defineEmits(['update:amount'])

const normalizeNumber = (value: string) =>
  value
    .replace(/[۰-۹]/g, (d) => String('۰۱۲۳۴۵۶۷۸۹'.indexOf(d)))
    .replace(/[^\d]/g, '')
    .slice(0, 8)

const belowThousand = (n: number): string => {
  const ones = ['', 'یک', 'دو', 'سه', 'چهار', 'پنج', 'شش', 'هفت', 'هشت', 'نه']
  const tens = ['', 'ده', 'بیست', 'سی', 'چهل', 'پنجاه', 'شصت', 'هفتاد', 'هشتاد', 'نود']
  const teens = [
    'ده',
    'یازده',
    'دوازده',
    'سیزده',
    'چهارده',
    'پانزده',
    'شانزده',
    'هفده',
    'هجده',
    'نوزده',
  ]
  const hundreds = ['', 'صد', 'دویست', 'سیصد', 'چهارصد', 'پانصد', 'ششصد', 'هفتصد', 'هشتصد', 'نهصد']

  let res: string[] = []
  if  (n >= 100) {
    res.push(hundreds[Math.floor(n / 100)]!)
    n %= 100
  }
  if (n >= 20) {
    res.push(tens[Math.floor(n / 10)]!)
    n %= 10
  } else if (n >= 10) return teens[n - 10]!
  if (n > 0) res.push(ones[n]!)
  return res.join(' و ')
}


const numberToWords = (num: number): string => {
  if (num === 0) return 'صفر تومان'
  const units = ['', 'هزار', 'میلیون', 'میلیارد']
  const parts: string[] = []
  let i = 0
  while (num) {
    const n = num % 1000
    if (n) parts.push(`${belowThousand(n)}${i ? ' ' + units[i] : ''}`)
    num = Math.floor(num / 1000)
    i++
  }
  return parts.reverse().join(' و ') + ' تومان'
}


const updateValue = (value: string) => {
  const clean = normalizeNumber(value)
  rawValue.value = clean
  errorMessage.value = ''

  if (!clean) {
    formattedValue.value = ''
    amountInWords.value = ''
    selectedAmount.value = null
    emit('update:amount', null) 
    return
  }

  const num = Number(clean)
  formattedValue.value = num.toLocaleString('fa-IR')
  amountInWords.value = numberToWords(num)
  selectedAmount.value = num

  emit('update:amount', num)
}

const onInput = (e: Event) => updateValue((e.target as HTMLInputElement).value)

const onKeyDown = (e: KeyboardEvent) => {
  const allowed = ['Backspace', 'Delete', 'ArrowLeft', 'ArrowRight', 'Tab']
  if (allowed.includes(e.key)) return
  if (!/[\d۰-۹]/.test(e.key)) {
    e.preventDefault()
    errorMessage.value = 'فقط عدد مجاز است'
  }
  if (rawValue.value.length >= 8) {
    e.preventDefault()
    errorMessage.value = 'حداکثر 8 رقم'
  }
}

const onPaste = (e: ClipboardEvent) => {
  const pasted = e.clipboardData?.getData('text') || ''
  if (!/^[\d۰-۹]+$/.test(pasted)) {
    e.preventDefault()
    errorMessage.value = 'چسباندن متن مجاز نیست'
  }
  if (normalizeNumber(pasted).length > 7) {
    e.preventDefault()
    errorMessage.value = 'حداکثر ۷ رقم'
  }
}

const selectPreset = (amount: number) => updateValue(amount.toString())
</script>

<template>
  <div class="max-w-md space-y-4">
    <div class="grid grid-cols-2 gap-3 mr-10 mt-8">
      <button
        v-for="amount in presetAmounts"
        :key="amount"
        @click="selectPreset(amount)"
        class="flex items-center justify-between px-4 py-3 transition"
      >
        <span
          class="flex h-5 w-5 items-center justify-center rounded-full border-2"
          :class="selectedAmount === amount ? 'border-blue-500 bg-white-500' : 'border-gray-400'"
        >
          <span v-if="selectedAmount === amount" class="h-3 w-3 rounded-full bg-blue-500 " />
        </span>
        <span class="text-gray-800">{{ amount.toLocaleString('fa-IR') }} تومان</span>
      </button>
    </div>

    <div
      class="flex items-center mr-10 rounded-lg border px-3 py-2 border-gray-300 hover:border-blue-400 focus-within:border-blue-500 focus-within:ring-1 focus-within:ring-blue-500 transition-all duration-200"
    >
      <input
        type="text"
        inputmode="numeric"
        class="flex-1 bg-transparent text-right outline-none"
        placeholder="حداکثر 8 رقم عددی"
        :value="formattedValue"
        @input="onInput"
        @keydown="onKeyDown"
        @paste="onPaste"
      />
      <span class="mr-2 text-gray-500">تومان</span>
    </div>

    <div v-if="errorMessage" class="mr-10 mb-1 font-bold text-sm text-red-500">
      {{ errorMessage }}
    </div>

    <div v-if="amountInWords" class="mr-10 text-sm text-gray-600">
      {{ amountInWords }}
    </div>
  </div>
</template>
