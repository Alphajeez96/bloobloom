<template>
  <div>
    <svg
      width="11"
      height="16"
      viewBox="0 0 11 16"
      fill="none"
      class="sidemenu-tab-icon"
    >
      <path
        d="M1 1L9 8.2L1 15.4"
        :stroke="isActive ? 'white' : 'black'"
        stroke-opacity="0.85"
        stroke-width="1.5"
      ></path>
    </svg>
  </div>
</template>
<script>
export default {
  props: {
    isActive: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
