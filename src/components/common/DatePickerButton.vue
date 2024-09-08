<template>
    <button :class="buttonClass" @click="handleClick">
      <slot></slot>
    </button>
  </template>
  
  <script setup>
  import { defineProps,  computed, defineEmits } from 'vue';
  
  // Define the props the component will receive
  const props = defineProps({
    // Dynamic classes can be passed as a string or array
    className: {
      type: [String, Array],
      default: ''
    },
    // Optional prop for additional attributes
    attributes: {
      type: Object,
      default: () => ({})
    }
  });
  
  // Define the emits the component will use
  const emit = defineEmits(['click']);
  
  // Computed property to handle dynamic classes
  const buttonClass = computed(() => {
    return [props.className];
  });
  
  const handleClick = () => {
    emit('click');
  };
  </script>
  
