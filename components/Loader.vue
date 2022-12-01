<template>
  <div
    element-loading-spinner="el-icon-loading"
    v-loading.fullscreen.lock="loading"
    element-loading-background="rgba(0, 0, 0, 0.8)"
  ></div>
</template>

<script>
export default {
  name: "Loader",
  props: {
    loading: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
