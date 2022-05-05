<template>
  <main id="container">
    <AppHeader />
    <AppRestaurantInfo :datasource="fooddata" />
  </main>
</template>

<script>
import { mapState } from "vuex";

import AppHeader from "@/components/AppHeader.vue";
import AppRestaurantInfo from "@/components/AppRestaurantInfo.vue";

export default {
  name: "IndexPage",
  components: {
    AppHeader,
    AppRestaurantInfo,
  },
  computed: {
    ...mapState(["fooddata"]),
  },
};
</script>

<style lang="scss" scoped></style>
