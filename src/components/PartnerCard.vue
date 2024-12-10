<template>
  <div class="container d-flex w-100 py-4">
    <div class="d-flex w-100 justify-content-between gap-4">

      <div class="partner__logo">
        <slot name="logo"></slot>
      </div>

      <div class="d-flex flex-column align-items-start">
        <span class="mb-4 fw-bolder text-start partner__name">{{ name }}</span>
        <div class="d-flex align-items-center gap-3 flex-wrap">
          <span>{{ phone }}</span>
          <span class="text-decoration-none">{{ email }}</span>
          <a :href="'https://' + website" target="_blank" class="text-decoration-none partner__link">
            {{ website }}
          </a>
        </div>
      </div>

      <button class="btn px-3 py-1 partner__status" :class="statusClass">{{ status }}</button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  phone: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  website: {
    type: String,
    required: true,
  },
  status: {
    type: String,
    required: true,
  },
})

// Классы для стилизации статуса партнера
const statusClass = computed(() => {
  if (props.status === 'Start') {
    return 'partner__status--start'
  } else if (props.status === 'Premier') {
    return 'partner__status--premier'
  }
  return '';
})
</script>

<style scoped>
.partner__name {
  font-size: 20px;
}

.partner__status {
  font-size: 12px;
  line-height: 15px;
  font-weight: 500;
  border-radius: 30px;
  height: max-content;
  color: var(--white);
}

.partner__status--start {
  background: var(--primary-gray);
}

.partner__status--premier {
  background: var(--primary-dark);
}

.partner__link {
  color: var(--primary-main);
}

.partner__logo {
  max-height: 70px;
}
</style>