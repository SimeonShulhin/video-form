<script setup>
import { computed } from 'vue';
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  value: {
    type: String,
    required: true,
  },
  modelValue: {
    type: String,
    required: true,
  },
});

const emits = defineEmits(['update:modelValue']);

const isChecked = computed(() => props.modelValue === props.value);

const updateValue = () => {
  emits('update:modelValue', props.value);
};
</script>

<template>
  <label class="border rounded p-2 cursor-pointer" :class="{ checked: isChecked }">
    <input
      type="radio"
      :name="name"
      :value="value"
      :checked="isChecked"
      @change="updateValue"
      class="hidden" />
    <slot></slot>
  </label>
</template>
