<template>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      :viewBox="'0 0 32 32'"
      :fill="color"
      :width="size"
      :height="size"
      aria-hidden="true"
      :class="['dp__icon', className]"
      role="img"
      aria-label="Down arrow icon"
    >
      <path
        d="M7.057 12.943l8 8c0.521 0.521 1.365 0.521 1.885 0l8-8c0.52-0.52 0.52-1.365 0-1.885s-1.365-0.52-1.885 0l-7.057 7.057c0 0-7.057-7.057-7.057-7.057-0.52-0.52-1.365-0.52-1.885 0s-0.52 1.365 0 1.885z"
      />
    </svg>
  </template>
  
  <script>
  export default {
    name: 'DownArrowIcon',
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
  /* Add any custom styles for the icon here if needed */
  </style>
  