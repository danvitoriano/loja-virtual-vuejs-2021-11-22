<template>
  <div>
    <b-row>
      <b-col lg="4" md="6">
        <b-card
          :title="title"
          :img-src="image"
          :img-alt="alt"
          img-top
          tag="article"
          class="my-3"
        >
          <b-card-text>
            <p v-if="inStock">Em estoque</p>
            <p v-else>Indisponível</p>
            <p>Entrega: {{ shipping }}</p>
          </b-card-text>
        </b-card>
      </b-col>
      <b-col lg="4" md="6">
        <b-card title="Detalhes" class="my-3">
          <ul>
            <li v-for="detail in details" :key="detail">{{ detail }}</li>
          </ul>
          <h4>Cores</h4>
          <b-badge
            :class="variant.variantColor"
            v-for="(variant, index) in variants"
            :key="variant.variantId"
            @mouseover="updateProduct(index)"
          >
            {{ variant.variantColor }}
          </b-badge>
          <hr />
          <b-button
            variant="primary"
            v-on:click="addToCart"
            :disabled="!inStock"
            :class="{ disabledButton: !inStock }"
          >
            Adicionar ao carrinho
          </b-button>
        </b-card>
        <b-card title="Reviews" tag="article" class="my-3">
          <p v-if="!reviews.length">There are no reviews yet.</p>
          <ul v-else>
            <li v-for="(review, index) in reviews" :key="index">
              <p>{{ review.name }}</p>
              <p>Rating: {{ review.rating }}</p>
              <p>{{ review.review }}</p>
              <p>{{ review.recommend }}</p>
            </li>
          </ul>
        </b-card>
      </b-col>
      <b-col lg="4" md="6">
        <b-card title="Avalie o produto" tag="article" class="my-3">
          <product-review @review-submitted="addReview"></product-review>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import ProductReview from "./ProductReview.vue";

export default {
  name: "product",
  components: {
    "product-review": ProductReview,
  },
  data() {
    return {
      product: "Meias Coloridas",
      brand: "Fiap",
      selectedVariant: 0,
      reviews: [],
      alt: "imagem de meias",
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          variantId: 2234,
          variantColor: "green",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
          variantQuantity: 10,
        },
        {
          variantId: 2235,
          variantColor: "blue",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
          variantQuantity: 0,
        },
      ],
    };
  },
  methods: {
    addToCart: function () {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].variantId);
    },
    updateProduct(index) {
      this.selectedVariant = index;
      console.log(index);
    },
    addReview(productReview) {
      this.reviews.push(productReview);
    },
  },
  computed: {
    title() {
      return this.brand + " " + this.product;
    },
    image() {
      return this.variants[this.selectedVariant].variantImage;
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity;
    },
    shipping() {
      if (this.premium) {
        return "Grátis";
      }
      return "R$ 2.99";
    },
  },
  props: {
    premium: {
      type: Boolean,
      required: true,
    },
  },
};
</script>

<style scoped>
.green {
  background-color: green;
}
.blue {
  background-color: blue;
}
</style>