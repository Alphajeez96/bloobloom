<template>
  <div>
    <div class="holder" v-if="!pending && glasses.length">
      <section class="w-full md:w-1/3"></section>

      <!-- Tilte Here -->
      <section class="w-full md:w-1/3 divider">
        <h1>{{ title }}</h1>
      </section>

      <!-- Available filters Here -->
      <section class="w-full md:w-1/3 filter__holder">
        <div class="inner__holder">
          <div class="filter" v-for="filter in filters" :key="filter">
            <a @click="isOpen = !isOpen"> {{ filter }}</a>
          </div>
        </div>
      </section>
    </div>

    <!-- Mobile Filter  -->
    <section class="mobile__filter__holder">
      <div class="inner__holder">
        <div class="filter" v-for="filter in filters" :key="filter">
          <a @click="isOpen = !isOpen"> {{ filter }}</a>
        </div>
      </div>
    </section>

    <!--Filter Display Here  -->
    <transition name="slide-fade">
      <section v-if="isOpen">
        <Filters
          :filters="filters"
          :length="glasses.length"
          @handleFilters="(data) => $emit('handleFilters', data)"
        />
      </section>
    </transition>

    <div v-if="pending">
      <Loader :loading="pending" />
    </div>

    <!-- Products Here -->
    <div class="mt-4" style="overflow: auto" v-else-if="glasses.length">
      <section class="flex">
        <div
          class="region__holder"
          v-infinite-scroll="load"
          :infinite-scroll-disabled="disabled"
          :infinite-scroll-immediate="false"
        >
          <div
            v-for="(glass, i) in glasses.slice(0, count)"
            :key="i"
            class="product__holder"
            :class="{ bordered: i % 3 == 0 }"
          >
            <!-- <img :src="glass.image" :alt="glass.name" loading="lazy" />
            <p>{{ glass.name }}</p> -->

            <el-carousel
              trigger="click"
              :autoplay="false"
              :arrow="glass.variants.length > 1 ? 'always' : 'never'"
              indicator-position="none"
            >
              <el-carousel-item v-for="item in glass.variants" :key="item.id">
                <img :src="item.media[0].url" :alt="glass.name" />
                <p>{{ glass.name }}</p>
              </el-carousel-item>
            </el-carousel>
          </div>
        </div>
      </section>

      <div v-if="loading">
        <Loader :loading="loading" />
      </div>
    </div>

    <!-- No Products Here -->
    <div class="empty" v-else>
      <h1>No Products</h1>
    </div>
  </div>
</template>

<script>
import Filters from "./filter";
export default {
  props: {
    title: {
      type: String,
      default: "Spectacles women",
    },

    loading: {
      type: Boolean,
      default: false,
    },

    pending: {
      type: Boolean,
      default: false,
    },

    count: {
      type: Number,
      default: 12,
    },

    glasses: {
      type: Array,
      default: () => [],
    },

    disabled: {
      type: Boolean,
      default: false,
    },

    load: {
      type: Function,
      default: () => null,
    },
  },

  components: { Filters },

  data: () => ({
    isOpen: false,
    filters: ["colour", "shape"],
  }),
};
</script>

<style lang="scss" scoped>
.holder {
  @apply h-[60px] flex flex-wrap md:flex-nowrap items-center justify-between  border-black border-t border-b-[0.5px];

  h1 {
    @apply text-center uppercase font-bold text-lg  md:text-[1.563rem] text-black;
  }

  .divider {
    @apply flex items-center justify-center h-full  border-black border-x;
  }

  .filter__holder {
    @apply hidden md:block h-full;

    .inner__holder {
      @apply flex items-center h-full;

      .filter {
        @apply w-[30%] h-full flex justify-center items-center;

        a {
          @apply text-sm uppercase cursor-pointer;
        }

        &:nth-child(2) {
          @apply border-x border-black;
        }
      }
    }
  }
}

.mobile__filter__holder {
  @apply block md:hidden h-12;

  .inner__holder {
    @apply flex justify-center items-center h-full border-black  border-l border-r;

    .filter {
      @apply w-[30%] h-full flex justify-center items-center;

      a {
        @apply text-sm uppercase cursor-pointer;
      }

      &:nth-child(2) {
        @apply border-l border-black;
      }
    }
  }
}

.region__holder {
  @apply flex flex-wrap w-full;
  .product__holder {
    @apply w-full md:w-1/2 xl:w-1/3 relative border-black border-[0.5px];

    p {
      @apply absolute top-[30px] left-[130px] md:left-[40%] font-medium text-black uppercase text-[15px] md:text-xl;
    }

    img {
      @apply object-contain;
    }
  }
}
.empty {
  @apply h-96 flex items-center justify-center flex-col;
}
</style>
