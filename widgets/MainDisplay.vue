<template>
  <div>
    <!-- Hero Image Here -->
    <section class="hero__section">
      <div class="desktop__hero" v-for="i in heroImages" :key="i + title">
        <img
          :src="require(`~/assets/images/hero-${i}.jpg`)"
          :alt="`hero${i}`"
          loading="lazy"
        />
      </div>

      <div class="mobile__hero" v-for="i in heroImages.slice(0, 1)" :key="i">
        <img
          :src="require(`~/assets/images/hero-${i}.jpg`)"
          :alt="`hero${i}`"
          loading="lazy"
        />
      </div>
    </section>

    <!-- Spectacles Here -->
    <section>
      <Spectacles
        :title="title"
        :load="load"
        :count="count"
        :loading="loading"
        :glasses="glasses"
        :disabled="disabled"
        :pending="pending"
        @handleFilters="handleFilters"
      />
    </section>
  </div>
</template>

<script>
import Spectacles from "~/widgets/Spectacles/index";
import filters from "~/plugins/filters";
import { debounce } from "lodash";

export default {
  name: "MainDisplay",
  components: { Spectacles },

  props: {
    url: {
      type: String,
      default: "",
      required: true,
    },

    heroImages: {
      type: Array,
      default: () => [1, 2],
    },

    title: {
      type: String,
      default: "spectacles women",
    },
  },

  data: () => ({
    loading: false,
    pending: false,
    count: 12,
    glasses: [],
    filters: [],
  }),

  computed: {
    noMore() {
      return this.count >= this.glasses.length;
    },
    disabled() {
      return this.loading || this.noMore;
    },
  },

  mounted() {
    this.fetchGlasses();
  },

  methods: {
    async fetchGlasses(filters) {
      let url = `${this.url}${filters ? filters : ""}`;

      try {
        this.pending = true;
        const response = await this.$axios.get(url);

        if (response.status === 200) {
          const { glasses } = response.data;

          this.glasses = glasses.map((element) => {
            return {
              id: element.id,
              name: element.name,
              variants: element.glass_variants,
            };
          });
        }
      } catch (error) {
        console.log("error::", error);
      } finally {
        this.pending = false;
      }
    },
    handleFilters(data) {
      let url = "";
      if (data.length) {
        data.forEach((item) => {
          const shape = filters.shapes.find((shape) => shape == item);
          const colour = filters.colours.find(
            (colour) => colour?.colour == item
          );

          if (shape) {
            url += `&filters[glass_variant_frame_variant_frame_tag_configuration_names][]=${shape}`;
          }

          if (colour) {
            url += `&filters[glass_variant_frame_variant_colour_tag_configuration_names][]=${colour.colour}`;
          }
        });

        this.debounceSearch(url);
      }
    },

    debounceSearch: debounce(function (url) {
      return this.fetchGlasses(url);
    }, 1500),

    load() {
      this.loading = true;
      setTimeout(() => {
        this.count = this.count + 12;
        this.loading = false;
      }, 1500);

      this.loading = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.hero__section {
  @apply flex flex-wrap md:flex-nowrap;

  .desktop__hero {
    @apply hidden md:block;
  }

  .mobile__hero {
    @apply block md:hidden;
  }
}
</style>
