<template>
  <div class="card">
    <img :src="product.imageUrl" alt="Denim Jeans" style="width:100%" />
    <h1>{{ product.title }}</h1>
    <p class="price">${{ product.price }}</p>
    <p>{{ product.description }}</p>
    <button>Add to Cart</button>
  </div>
</template>

<script>
import { groq } from "@nuxtjs/sanity";

export default {
  async asyncData({ params, $sanity }) {
    const query = groq`
    *[_type == "product" && slug.current == "${params.slug}"][0]{
      title,
      "price": defaultProductVariant.price,
      "description": body.en[0].children[0].text,
      "imageUrl": defaultProductVariant.images[0].asset->url
    }`;
    const product = await $sanity.fetch(query);
    return { product };
  }
};
</script>

<style>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  max-width: 500px;
  margin: auto;
  text-align: center;
  font-family: arial;
}

.price {
  color: grey;
  font-size: 22px;
  margin: 1rem;
}

.card button {
  border: none;
  outline: 0;
  padding: 12px;
  color: white;
  background-color: #000;
  text-align: center;
  cursor: pointer;
  width: 100%;
  font-size: 18px;
  margin-top: 3rem;
}

.card button:hover {
  opacity: 0.7;
}
</style>
