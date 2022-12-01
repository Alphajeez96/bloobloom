<template>
  <div class="sub__menu">
    <div class="nav">
      <a @click="$emit('goBack')"> go back</a>
    </div>

    <ul>
      <li v-for="item in menu" :key="item">
        <a @click="handleRoute(item)">{{ item }}</a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SubMenu",

  props: {
    route: {
      type: String,
      default: "",
    },
  },

  data: () => ({
    menu: ["women", "men"],
  }),

  methods: {
    handleRoute(item) {
      if (item === "women" && this.route === "spectacles") {
        this.$router.push("/");
      } else {
        this.$router.push(`/${this.route}/${item}`);
      }
      this.$emit("undoRoute");
    },
  },
};
</script>

<style lang="scss" scoped>
.sub__menu {
  @apply w-72  h-screen border-r  border-black bg-white transition-all duration-500;

  ul {
    a {
      @apply text-sm uppercase font-medium cursor-pointer p-5 border-b border-black bg-inherit flex items-center;

      &:hover {
        @apply bg-black text-white;
      }
    }
  }
  .nav {
    @apply flex justify-end items-center p-5 border-b border-black;

    a {
      @apply cursor-pointer uppercase font-semibold;
    }
  }
}
</style>
