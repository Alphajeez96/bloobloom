<template>
  <header>
    <nav>
      <!-- Sidebar Here -->
      <template>
        <aside class="absolute z-10 dropdown">
          <SideBar @handleRoute="(route) => (activeRoute = route)" />
        </aside>

        <aside class="absolute z-50" :class="{ is__open: activeRoute }">
          <SubMenu
            :route="activeRoute"
            @goBack="activeRoute = ''"
            @undoRoute="undoRoute"
          />
        </aside>
      </template>

      <!-- Menu Here -->
      <div class="w-1/3">
        <a class="uppercase cursor-pointer text-xs tracking-[2px]">menu</a>
      </div>

      <!-- Logo Here -->
      <div class="w-1/3">
        <Logo />
      </div>

      <div></div>
    </nav>
  </header>
</template>

<script>
import SideBar from "~/widgets/Partials/side-bar";
import SubMenu from "~/widgets/Partials/sub-menu";
export default {
  components: { SideBar, SubMenu },

  data: () => ({
    isOpen: false,
    activeRoute: "",
  }),

  methods: {
    undoRoute() {
      this.activeRoute = "";
      this.isOpen = false;
    },
  },
};
</script>

<style lang="scss" scoped>
header {
  @apply bg-white flex items-center  fixed  top-0 border border-black h-14 w-full z-50 px-4 md:px-14;

  nav {
    @apply flex items-center relative justify-start md:justify-between w-full;

    &:hover {
      .dropdown {
        @apply opacity-100 -left-[57px];
      }
    }
  }
}

aside {
  @apply top-10 -left-[447px] z-[50px] opacity-0 transition-all duration-500;

  &.is__open {
    @apply opacity-100 -left-[57px];
  }
}
</style>
