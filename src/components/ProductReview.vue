<template>
  <div>
    <b-form @submit.prevent="onSubmit">
      <p class="error" v-if="errors.length">
        <strong>corrija os seguintes erros</strong>
        <ul>
            <li v-for="error in errors" :key="error">{{error}}</li>
        </ul>
      </p>
      <b-form-group id="input-group-1" label="Nome:" label-for="name">
        <b-form-input
          id="name"
          v-model="name"
          type="text"
          placeholder="Seu nome"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Review:" label-for="review">
        <b-form-textarea
          id="review"
          v-model="review"
          placeholder="Enter something..."
          rows="3"
          max-rows="6"
        >
        </b-form-textarea>
      </b-form-group>
      <b-form-group id="input-group-3" label="Rating:" label-for="rating">
        <b-form-rating v-model="rating"></b-form-rating>
      </b-form-group>
      <b-form-group label="Você recomendaria esse produto?">
        <div style="display: flex">
          <b-form-radio button v-model="recommend" value="sim">
            Sim</b-form-radio
          >
          <b-form-radio button v-model="recommend" value="não" class="px-2">
            Não</b-form-radio
          >
        </div>
      </b-form-group>
      <b-button variant="success" type="submit" class="my-3">Enviar</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  name: "product-review",
  data() {
    return {
      name: null,
      review: null,
      rating: null,
      recommend: null,
      errors: [],
    };
  },
  methods: {
    onSubmit() {
      this.errors = [];
      if (this.name && this.review && this.rating) {
        let productReview = {
          name: this.name,
          review: this.review,
          rating: this.rating,
          recommend: this.recommend,
        };
        this.$emit("review-submitted", productReview);
        (this.name = null),
          (this.review = null),
          (this.rating = null),
          (this.recommend = null);
      } else {
        if (!this.name) this.errors.push("Nome obrigatorio");
        if (!this.review) this.errors.push("Review obrigatorio");
        if (!this.rating) this.errors.push("Rating obrigatorio");
        if (!this.recommend) this.errors.push("Recomendação obrigatorio");
      }
    },
  },
};
</script>
