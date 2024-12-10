<template>
  <div class="w-100">
    <select class="form-select py-3 px-3 filter-select" @change="onChange" :value="modelValue">
      <option value disabled selected hidden>{{ name }}</option>
      <option
        v-for="(option, index) in options"
        :key="index"
        :value="option"
      >
        {{ option }}
      </option>
    </select>
  </div>
</template>

<script setup>
// Отображаемое название селекта, опции селекта, modelValue для изменения
const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  options: {
    type: Array,
    required: true,
  },
  modelValue: {
    type: String,
    required: true,
  },
})

// Обновление значения в родителе
const emit = defineEmits(['update:modelValue'])

const onChange = (e) => {
  emit('update:modelValue', e.target.value)
}
</script>

<style scoped>
.filter-select {
  color: var(--secondary-gray);
}
</style>