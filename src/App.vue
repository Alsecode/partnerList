<template>
  <div class="container">
    <div class="row mb-5">
      <h1 class="h1 mb-4 heading-main">Где купить</h1>
      <h2 class="h2 heading-secondary">Контакты наших партнёров</h2>
    </div>

    <div class="row gx-5 gy-5">
      <div class="col-12 col-lg-4">
        <FiltersMain
          v-model:country="selectedCountry" 
          v-model:city="selectedCity"
          v-model:productType="selectedProductType"
          v-model:product="selectedProduct"
          v-model:partnerType="selectedPartnerType"
        />
        <div class="d-flex gap-3 flex-wrap mt-5">
          <button class="rounded-1 button button--clear" @click="clear">Очистить</button>
          <button class="rounded-1 button button--search" @click="search">Найти</button>
        </div>
        <p v-if="isError" class="mt-2 text-start error">
          Ошибка! Выбраны не все обязательные фильтры: адрес, тип продукта, продукт
        </p>
      </div>
      <div class="col">
        <PartnerCard
          v-for="(partner, index) in filteredPartners"
          :key="partner.id"
          :name="partner.name"
          :phone="partner.phone"
          :email="partner.email"
          :website="partner.website"
          :status="partner.status"
          class="partner-card"
        >
          <template v-slot:logo>
            <img v-if="index === 1 || index === 3" class="img" src="./assets/img/ecoPack.png" />
            <img v-else class="img" src="./assets/img/algrum.png" />
          </template>
        </PartnerCard>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import partnersData from './partnersData.json'
import PartnerCard from './components/PartnerCard.vue'
import FiltersMain from './components/Filters/FiltersMain.vue'

const filteredPartners = ref([...partnersData])

// Ошибка выбора не всех фильтров
const isError = ref(false)

// Выбранная страна
const selectedCountry = ref('')
// Выбранный город
const selectedCity = ref('')
// Выбранный тип продукта
const selectedProductType = ref('')
// Выбранный продукт
const selectedProduct = ref('')
// Выбранный тип партнёра
const selectedPartnerType = ref([])

// Функция проверки обязательных фильтров
const allRequiredSelected = () => {
  return selectedCountry.value && selectedCity.value && selectedProductType.value && selectedProduct.value
}

// Функция применения фильтров
const search = () => {
  // Проверка, выбраны ли обязательные поля
  if (!allRequiredSelected()) {
    isError.value = true
    return
  }

  isError.value = false

  filteredPartners.value = partnersData.filter(partner => {
    // Проверка обязательные фильтры
    const countryMatch = partner.country === selectedCountry.value
    const cityMatch = partner.city === selectedCity.value
    const productTypeMatch = partner.productType === selectedProductType.value
    const productMatch = partner.products.includes(selectedProduct.value)

    // Проверка типа партнёра (если выбран)
    const partnerTypeMatch = selectedPartnerType.value.length === 0
      ? true
      : selectedPartnerType.value.some(pt => partner.partnerType.includes(pt))

    return countryMatch && cityMatch && productTypeMatch && productMatch && partnerTypeMatch
  })
}

// Функция очистки фильтров
const clear = () => {
  // Сброс выбранных значений
  selectedCountry.value = ''
  selectedCity.value = ''
  selectedProductType.value = ''
  selectedProduct.value = ''
  selectedPartnerType.value = []

  filteredPartners.value = [...partnersData]
}
</script>

<style>
:root {
  --white: #ffffff;
  --primary-main: #00a5ad;
  --primary-dark: #26263a;
  --primary-gray: #9999a2;
  --secondary-gray: #666674;
  --secondary-light-gray: #e6e6e8;
  --border-color: #ccccd1;
  --error: #e94141;
}

@font-face {
  font-family: 'Gogh';
  font-weight: 400;
  src: url('./assets/fonts/Gogh-Regular.ttf');
}

@font-face {
  font-family: 'Gogh';
  font-weight: 500;
  src: url('./assets/fonts/Gogh-Medium.ttf');
}

@font-face {
  font-family: 'Gogh';
  font-weight: 800;
  src: url('./assets/fonts/Gogh-ExtraBold.ttf');
}

* {
  font-family: 'Gogh';
}

.heading-main {
  font-size: 46px;
  font-weight: 800;
}

@media screen and (max-width: 992px) {
  .heading-main {
    font-size: 28px;
    margin-bottom: 16px;
  }
}

.heading-secondary {
  font-size: 20px;
  font-weight: 400;
}

.img {
  max-height: 100%;
}

.partner-card {
  border-bottom: 1px solid var(--border-color);
}

.partner-card:last-child {
  border: none;
}

.button {
  padding: 12px 48px;
  color: var(--primary-dark);
  font-size: 18px;
  line-height: 22px;
  font-weight: 500;
  background: none;
  outline: none;
  border: 0;
  transition: 0.2s all;
}

.button:hover {
  opacity: 0.7;
}

.button--clear {
  border: 2px solid var(--primary-dark);
}

.button--search {
  color: var(--white);
  background: var(--primary-main);
}

.error {
  font-size: 12px;
  color: var(--error);
}
</style>