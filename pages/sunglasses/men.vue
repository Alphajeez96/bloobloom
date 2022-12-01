<template>
  <div>
    <MainDisplay :url="url" :heroImages="heroImages" title="sunglasses men" />
  </div>
</template>

<script>
import MainDisplay from "~/widgets/MainDisplay.vue";

export default {
  name: "WomenSpectacles",
  components: { MainDisplay },

  data: () => ({
    url: "https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections/sunglasses-men/glasses?sort[type]=collection_relations_position&sort[order]=asc&filters[lens_variant_prescriptions][]=fashion&filters[lens_variant_types][]=classic&filters[frame_variant_home_trial_available]=false",
    heroImages: [7, 8],
  }),
};
</script>
