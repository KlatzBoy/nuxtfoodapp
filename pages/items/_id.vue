<template>
  <section class="restaurantinfo">
    <div class="item row">
      <section class="image">
        <img
          :src="require(`~/static/${currentItem.img}`)"
          :alt="currentItem.description"
        />
      </section>
      <section class="details">
        <h1>{{ currentItem.item }}</h1>
        <h2>Price: ${{ currentItem.price.toFixed(2) }}</h2>
        <div class="quantity">
          <input v-model.number="count" type="number" step="1" />
          <button @click="addToCart">Add to Cart: $ {{ combinedPrice }}</button>
        </div>

        <fieldset v-if="currentItem.options">
          <legend>
            <h3>options</h3>
          </legend>
          <div v-for="option in currentItem.options" :key="option">
            <input
              type="radio"
              name="option"
              :id="option"
              :value="option"
              v-model="itemOptions"
            />
            <label :for="option">{{ option }}</label>
          </div>
        </fieldset>

        <fieldset v-if="currentItem.addOns">
          <legend>
            <h3>Addons</h3>
          </legend>
          <div v-for="addon in currentItem.addOns" :key="addon">
            <input
              type="checkbox"
              name="addon"
              :id="addon"
              :value="addon"
              v-model="itemAddons"
            />
            <label :for="addon">{{ addon }}</label>
          </div>
        </fieldset>
        <AppToast v-if="cartSubmitted">
          Order Submitted <br />
          Check out more
          <nuxt-link to="/restaurants">Restaurants</nuxt-link>
        </AppToast>
      </section>
      <section class="description">
        <h3>Description</h3>
        <p>{{ currentItem.description }}</p>
      </section>
    </div>
  </section>
</template>

<script>
import { mapState } from "vuex";
import AppToast from "@/components/AppToast.vue";

export default {
  data() {
    return {
      id: this.$route.params.id,
      count: 1,
      itemOptions: "",
      itemAddons: [],
      itemSizeAndCost: [],
      cartSubmitted: false,
    };
  },
  components: {
    AppToast,
  },
  computed: {
    ...mapState(["fooddata", "cart"]),
    currentItem() {
      let result;
      for (let i = 0; i < this.fooddata.length; i++) {
        for (let j = 0; j < this.fooddata[i].menu.length; j++) {
          if (this.fooddata[i].menu[j].id === this.id) {
            result = this.fooddata[i].menu[j];
            break;
          }
        }
      }
      return result;
    },
    combinedPrice() {
      let total = this.count * this.currentItem.price;
      return total.toFixed(2);
    },
  },
  methods: {
    addToCart() {
      let formOutput = {
        item: this.currentItem.item,
        count: this.count,
        options: this.itemOptions,
        addOns: this.itemAddons,
        combinedPrice: this.combinedPrice,
      };

      this.$store.commit("addToCart", formOutput);
      this.cartSubmitted = true;
    },
  },
};
</script>

<style lang="scss" scoped>
.item {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-column-gap: 10px;
  grid-row-gap: 10px;
}

.image {
  grid-area: 1 / 1 / 2 / 2;
}
.details {
  grid-area: 1 / 2 / 2 / 3;
  position: relative;
}
.description {
  grid-area: 2 / 1 / 3 / 3;
}
h1,
h2,
label {
  font-family: "Poppins", sans-serif;
}
img {
  width: 100%;
  object-fit: cover;
}
</style>
