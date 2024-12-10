<template>
  <div class="btn-group d-flex flex-column align-items-start" role="group">
    <span class="mb-3 select-button__label">{{ label }}</span>
    <div class="d-flex flex-wrap gap-2">
      <div v-for="(option, index) in options" :key="`${option}${index}`">
        <input 
          v-if="Array.isArray(modelValue)" 
          type="checkbox" 
          class="btn-check" 
          :id="`${option}${index}`"
          :value="option"
          :checked="modelValue.includes(option)"
          @change="toggleOption(option)"
        >
        <input 
          v-else 
          type="radio" 
          class="btn-check" 
          :id="`${option}${index}`" 
          :value="option" 
          :checked="modelValue === option"
          @change="selectOption(option)"
        >
        <label 
          class="border-0 px-3 py-2 select-button"
          :for="`${option}${index}`"
          :class="{
            'select-button--active':
              (Array.isArray(modelValue) && modelValue.includes(option)) ||
              (!Array.isArray(modelValue) && modelValue === option)
          }"
        >
          {{ option }}
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
// Отображаемый лейбл селекта, опции селекта, modelValue для изменения
const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  options: {
    type: Array,
    required: true,
  },
  modelValue: {
    type: [String, Array],
    required: true,
  },
})

// Обновление значения в родителе
const emit = defineEmits(['update:modelValue'])

// Изменение выбора опции (при множественном выборе)
const toggleOption = (option) => {
  if (!Array.isArray(props.modelValue)) return
  const newValue = [...props.modelValue]
  const index = newValue.indexOf(option)
  
  if (index === -1) {
    newValue.push(option)
  } else {
    newValue.splice(index, 1)
  }
  
  emit('update:modelValue', newValue)
}

// Изменение выбора опции (при единственном выборе)
const selectOption = (option) => {
  if (Array.isArray(props.modelValue)) return
  emit('update:modelValue', option)
}
</script>

<style scoped>
.select-button__label {
  font-size: 14px;
  color: var(--primary-gray);
}

.select-button {
  border-radius: 30px;
  font-size: 14px;
  line-height: 17px;
  color: var(--primary-dark);
  background: var(--secondary-light-gray);
  cursor: pointer;
}

.select-button--active {
  color: var(--white);
  background: var(--primary-dark);
}
</style>