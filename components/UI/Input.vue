<template>
  <input
      placeholder=" "
      ref="input"
      :value="modelValue"
      @change="emitData($event.target.value)"
      />
</template>

<script setup>
const input = ref()
const props = defineProps([
  'modelValue',
])

const emit = defineEmits(['update:modelValue'])

function emitData(value) {
  emit('update:modelValue', value)
}
</script>

<style scoped>

</style>