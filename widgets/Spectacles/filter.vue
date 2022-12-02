<template>
  <div class="mt-4">
    <!-- Filter Panel Here -->
    <section class="panel__region">
      <div class="panel__holder" v-for="filter in filters" :key="filter">
        <p>{{ filter }}</p>
      </div>
    </section>

    <!-- Filter Panel Here -->
    <section class="panel__content">
      <!-- Colours here -->
      <div class="content__holder">
        <div class="content" v-for="(colour, i) in colours" :key="i">
          <div @click="handleColourSelection(i)">
            <span
              class="pill__holder"
              :class="{ bordered: selected.includes(colour.colour) }"
            >
              <span
                class="pill"
                :style="{ background: `url(${colour.url})` || 'black' }"
              ></span>
            </span>
            <p>{{ colour.colour }}</p>
          </div>
        </div>
      </div>

      <!-- Shapes here -->
      <div class="content__holder justify__around">
        <a
          v-for="shape in shapes"
          :key="shape"
          @click="handleShapeSelection(shape)"
          :class="{ underlined: selected.includes(shape) }"
        >
          {{ shape }}
        </a>
      </div>
    </section>

    <!-- Search Results -->
    <section class="search__result">
      <!-- FIlter display -->
      <div class="result__holder filter">
        <div v-for="selection in selected" :key="selection">
          <p>{{ selection }}</p>
          <span @click="removeSelection(selection)">
            <Icon />
          </span>
        </div>
      </div>

      <!-- Result display -->
      <div class="result__holder result">
        {{ `${length} ${length == 1 ? "result" : "results"} found` }}
      </div>
    </section>
  </div>
</template>

<script>
import Icon from "./delete-icon.vue";
import filters from "~/plugins/filters";
export default {
  props: {
    filters: {
      type: Array,
      default: () => [],
    },

    length: {
      type: [String, Number],
      default: "",
    },
  },

  components: { Icon },

  data: () => ({
    selected: [],
    shapes: [],
    colours: [],
  }),

  mounted() {
    const { colours, shapes } = filters;
    this.colours = colours;
    this.shapes = shapes;
  },

  methods: {
    handleColourSelection(index) {
      const colour = this.colours[index];
      const found = this.selected.find((item) => item == colour.colour);

      if (found) {
        this.selected = this.selected.filter((item) => item != colour.colour);
      } else {
        this.selected.push(colour.colour);
      }
      this.$emit("handleFilters", this.selected);
    },

    handleShapeSelection(shape) {
      const found = this.selected.find((item) => item == shape);

      if (found) {
        this.selected = this.selected.filter((item) => item != shape);
      } else {
        this.selected.push(shape);
      }
      this.$emit("handleFilters", this.selected);
    },

    removeSelection(selection) {
      this.selected = this.selected.filter((item) => item != selection);
      this.$emit("handleFilters", this.selected);
    },
  },
};
</script>

<style lang="scss" scoped>
.panel__region {
  @apply h-[60px] hidden md:flex items-center;

  .panel__holder {
    @apply w-1/3 flex items-center justify-center h-[60px] border-r border-black;

    p {
      @apply uppercase font-bold text-[15px];
    }
  }
}
.panel__content {
  @apply h-full md:h-[120px] flex flex-wrap items-center border-t border-x border-black;

  .content__holder {
    @apply w-full md:w-1/2 lg:w-1/3 flex  flex-wrap items-center md:h-[120px] border-r border-black px-4 md:px-8 py-3.5;

    &.justify__around {
      @apply justify-around border-t border-black md:border-t-0;

      a {
        @apply capitalize text-base font-normal cursor-pointer;

        &.underlined {
          @apply underline;
        }

        &:hover {
          @apply underline;
        }
      }
    }

    .content {
      @apply flex items-center w-1/2 md:w-1/3 flex-wrap;

      div {
        @apply flex items-center cursor-pointer;

        &:hover {
          .pill__holder {
            @apply border-black border;
          }
        }

        .pill__holder {
          @apply w-[18px] h-[18px] rounded-full mr-2.5;

          &.bordered {
            @apply border-black border;
          }
        }

        .pill {
          @apply h-4 w-4 rounded-full block;
        }

        p {
          @apply capitalize pt-0.5 font-normal;
        }
      }
    }
  }
}

.search__result {
  @apply flex items-center  w-full min-h-[60px] justify-center  md:justify-between border-x  border-t border-black py-3.5;

  .result__holder {
    @apply md:w-7/12 uppercase;

    &.filter {
      @apply w-1/3 hidden md:flex items-center gap-5 flex-wrap px-8;

      div {
        @apply flex items-center;

        span {
          @apply cursor-pointer;
        }
      }

      p {
        @apply text-[13px] pt-1 mr-2.5;
      }
    }

    &.result {
      @apply font-semibold;
    }
  }
}
</style>
