<template>
  <div id="Restaurants" class="container restaurant">
    <div class="restaurantheading">
      <h1>Our restaurants</h1>

      <AppSelect @change="selectedRestaurant = $event" />
    </div>
    <AppRestaurantInfo :datasource="filteredRestaurants" />
  </div>
</template>

<script>
import { mapState } from "vuex";

import AppRestaurantInfo from "@/components/AppRestaurantInfo.vue";
import AppSelect from "@/components/AppSelect.vue";

export default {
  components: {
    AppRestaurantInfo,
    AppSelect,
  },
  data() {
    return {
      selectedRestaurant: "",
    };
  },
  computed: {
    ...mapState(["fooddata"]),
    filteredRestaurants() {
      if (this.selectedRestaurant) {
        return this.fooddata.filter((el) => {
          let name = el.name.toLowerCase();
          return name.includes(this.selectedRestaurant);
        });
      }
      return this.fooddata;
    },
  },
};
</script>
