<template>
  <div class="container">
    <h1 class="title">Products</h1>
    <div class="products">
      <div v-for="product in products" :key="product._id" class="products">
        <h2>
          <a :href="product.slug.current" v-text="product.title" />
        </h2>
      </div>
    </div>
  </div>
</template>

<script>
import { groq } from "@nuxtjs/sanity";
export default {
  async asyncData({ $sanity }) {
    const query = groq`*[_type == "product"]`;
    const products = await $sanity.fetch(query);

    return { products };
  }
};
</script>

<style>
.container {
  min-height: 100vh;
  display: block;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
  margin-bottom: 2rem;
}

.products {
  padding-top: 15px;
  text-decoration: none;
  display: block;
  font-weight: 300;
  font-size: 20px;
  color: #115196;
  letter-spacing: 1px;
}
a {
  text-decoration: none;
}
</style>
