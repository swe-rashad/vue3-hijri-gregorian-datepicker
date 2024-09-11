<template>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      :viewBox="'0 0 32 32'"
      :fill="color"
      :width="size"
      :height="size"
      aria-hidden="true"
      :class="['dp__icon',className]"
      role="img"
      aria-label="Back arrow icon"
    >
      <path
        d="M20.943 23.057l-7.057-7.057c0 0 7.057-7.057 7.057-7.057 0.52-0.52 0.52-1.365 0-1.885s-1.365-0.52-1.885 0l-8 8c-0.521 0.521-0.521 1.365 0 1.885l8 8c0.52 0.52 1.365 0.52 1.885 0s0.52-1.365 0-1.885z"
      />
    </svg>
  </template>
  
  <script>
  export default {
    name: 'BackArrowIcon',
    props: {
      className: {
        type: String,
        default: '',
      },
      size: {
        type: Number,
        default: 32,
      },
      color: {
        type: String,
        default: 'currentColor',
      },
    },
  };
  </script>
  
  <style scoped>
  /* Custom styles can be added here if needed */
  </style>
  