<template>
  <div class="card">
    <img :src="product.imageUrl" alt="Product image" style="width:100%" />
    <h1>{{ product.title }}</h1>
    <p class="price">${{ product.price }}</p>
    <p>{{ product.blurb }}..</p>
    <p><button>Add to Cart</button></p>
  </div>
</template>

<script>
import { groq } from "@nuxtjs/sanity";
export default {
  async asyncData({ params, $sanity }) {
    const query = groq`*[_type == "product" && 
        slug.current == "${params.slug}"
        ][0]{
        title,
        "price": defaultProductVariant.price,
        "imageUrl": defaultProductVariant.images[0].asset->url,
        "blurb": blurb.en
        }`;
    const product = await $sanity.fetch(query);

    return { product };
  }
};
</script>

<style>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  max-width: 300px;
  margin: auto;
  text-align: center;
  font-family: arial;
}

.price {
  color: grey;
  font-size: 22px;
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
}

.card button:hover {
  opacity: 0.7;
}
</style>
