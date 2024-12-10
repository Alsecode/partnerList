<template>
  <div class="d-flex flex-column gap-4">
    <div class="d-flex flex-column align-items-start w-100">
      <span class="mb-4 filter__adress">Уточните адрес</span>
      <FilterSelect :options="countries" name="Страна" v-model="countryValue" class="mb-3" />
      <FilterSelect :options="cities" name="Город" v-model="cityValue" />
    </div>

    <FilterButtons :options="productTypes" label="Выберите тип продукта" v-model="productTypeValue" />
    <FilterButtons :options="products" label="Выберите продукт" v-model="productValue" />
    <FilterButtons :options="partnerTypes" label="Выберите тип партнёра" v-model="partnerTypeValue" />
  </div>
</template>

<script setup>
import { computed } from 'vue';
import FilterButtons from './FilterButtons.vue'
import FilterSelect from './FilterSelect.vue'

// Страна, город, тип продукта, продукт, тип партнёра
const props = defineProps({
  country: {
    type: String,
    required: true,
  },
  city: {
    type: String,
    required: true,
  },
  productType: {
    type: String,
    required: true,
  },
  product: {
    type: String,
    required: true,
  },
  partnerType: {
    type: Array,
    required: true,
  },
})

const emits = defineEmits([
  'update:country',
  'update:city',
  'update:productType',
  'update:product',
  'update:partnerType'
])

const countryValue = computed({
  get: () => props.country,
  set: val => emits('update:country', val)
})

const cityValue = computed({
  get: () => props.city,
  set: val => emits('update:city', val)
})

const productTypeValue = computed({
  get: () => props.productType,
  set: val => emits('update:productType', val)
})

const productValue = computed({
  get: () => props.product,
  set: val => emits('update:product', val)
})

const partnerTypeValue = computed({
  get: () => props.partnerType,
  set: val => emits('update:partnerType', val)
})

const countries = ['Россия', 'Другая']

const cities = ['Москва', 'Санкт-Петербург', 'Новосибирск']

const productTypes = ['Для дома', 'Для бизнеса']

const products = ['Антивирус', 'GetScreen', 'Passwork']

const partnerTypes = ['Партнёры Retail', 'Партнёры Corporate', 'Интернет-провайдеры', 'Online партнёры',
  'Продажи партнёрам', 'Education партнёры', 'MSP Партнёры'
]
</script>

<style scoped>
.filter__adress {
  font-size: 14px;
  color: var(--primary-gray);
}
</style>