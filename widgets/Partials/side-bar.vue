<template>
  <div class="holder">
    <ul>
      <li
        v-for="(route, i) in routes"
        :key="i"
        @mouseenter="isActive = i"
        @mouseleave="isActive = null"
        @click="handleRoute(route)"
        :class="{ active: activeRoute === route, disabled: i > 1 }"
      >
        {{ route }}

        <span v-if="i < 2">
          <Arrow :isActive="isActive == i || activeRoute === route" />
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
import Arrow from "./arrow.vue";
export default {
  name: "SideBar",
  components: { Arrow },

  data: () => ({
    activeRoute: "",
    isActive: null,
    isSideMenu: false,
    routes: ["spectacles", "sunglasses", "home try on", "pair for pair "],
  }),

  methods: {
    handleRoute(route) {
      this.activeRoute = route;
      this.$emit("handleRoute", route);
    },
  },
};
</script>

<style lang="scss" scoped>
.holder {
  @apply w-72  h-screen border-r border-l border-black bg-white;

  ul {
    li {
      @apply text-sm uppercase font-medium cursor-pointer p-5 border-b border-black bg-inherit flex items-center;

      &.active {
        @apply bg-black text-white;
      }

      &:hover {
        @apply bg-black text-white;
      }

      &.disabled {
        @apply pointer-events-none bg-white text-black;
      }

      span {
        @apply ml-auto;
      }
    }
  }
}
</style>
