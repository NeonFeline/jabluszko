<script setup>
import { ref } from "vue";
const props = defineProps(["category"]);

const productsArray = ref(undefined);

// Get the current year
const date = new Date().getFullYear();

// Function to load the products (Note that this is an async function)
const loadProduct = async function () {
  const json = await fetch("/products.json");
  const data = await json.json();
  return data;
};

// Load the products
productsArray.value = await loadProduct();
</script>

<template>
  <div class="main_selection">
    <!-- Display every product in the category in a grid -->
    <div class="main_selection__grid">
      <!-- Product template -->
      <div
        class="main_selection__product"
        v-for="product in productsArray[props.category]"
      >
        <img :src="product.src" alt="" class="main_selection__product__image" />
        <h4 class="main_selection__product__name">
          {{ product.name }}
        </h4>
        <span class="main_selection__product__price"
          >{{ product.price }}zł/kg</span
        >
      </div>
    </div>

    <footer class="main_selection__footer">{{ date }}</footer>
  </div>
</template>

<style scoped lang="scss">
.main_selection {
  background-color: var(--color-secondary);
  width: 100%;

  &__grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
  }

  &__product {
    padding: 2.4rem 4.8rem;
    height: 25rem;

    &__image {
      height: 12rem;
      width: 12rem;
    }

    &__name {
    }

    &__price {
    }
  }

  &__footer {
  }
}
</style>
