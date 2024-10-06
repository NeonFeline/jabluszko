<script setup>
const PRODUCT_ROW_COUNT = 3;
const PRODUCT_COLUMN_COUNT = 6;
const PRODUCT_COUNT = PRODUCT_ROW_COUNT * PRODUCT_COLUMN_COUNT;

import { ref, watch } from "vue";

// Sleep function for animation
function sleep(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

const props = defineProps(["category"]);
const emit = defineEmits(["selectedProduct"]);

const productsArray = ref(undefined);
const categoryRef = ref("owoce");

// Get the current year
const date = new Date().getFullYear();

// Function to load the products (Note that this is an async function)
const loadProduct = async function () {
  const json = await fetch("/products.json");
  const data = await json.json();
  return data;
};

// Current page
const page = ref(0);
const animationSwipeProductsActive = ref(false);

// Watch if the category has been changed
watch(props, async (category) => {
  animationSwipeProductsActive.value = true;
  page.value = -1;
  await sleep(250);
  page.value = 0;
  animationSwipeProductsActive.value = false;
});

// Function to format currency
let zloty = Intl.NumberFormat("pl-PL", {
  style: "currency",
  currency: "PLN",
});

// Sends information that a product has been selected to the parent
const emitProduct = function (product) {
  emit("selectedProduct", product);
};

const changePage = async function (newPage) {
  animationSwipeProductsActive.value = true;
  await sleep(250);
  page.value = newPage;
  animationSwipeProductsActive.value = false;
};

// Load the products
productsArray.value = await loadProduct();
</script>

<template>
  <div class="main_selection">
    <!-- dummy wraper -->
    <div class="main_selection__wrapper main_selection__wrapper--outer">
      <div
        :class="{
          main_selection__wrapper: true,
          'main_selection__wrapper--inner': true,
          'main_selection__wrapper--inner--animation': animationSwipeProductsActive,
        }"
      >
        <!-- Display every product in the category in a grid -->
        <div class="main_selection__grid">
          <!-- Product template -->
          <div
            class="main_selection__product"
            v-for="product in productsArray[props.category].slice(
              page * PRODUCT_COUNT,
              (page + 1) * PRODUCT_COUNT
            )"
            @click="emitProduct(product)"
          >
            <!-- Product image -->
            <img
              :src="product.src"
              alt=""
              class="main_selection__product__image"
            />

            <!-- Product name -->
            <h4 class="main_selection__product__name">
              {{ product.name }}
            </h4>

            <!-- Product price -->
            <span class="main_selection__product__price"
              >{{ zloty.format(product.price) }}/kg</span
            >
          </div>
        </div>
      </div>
    </div>

    <!-- PAGINATION -->
    <div class="pagination">
      <!-- Left button -->
      <button
        class="pagination_btn pagination_btn--left"
        @click="changePage(page - 1)"
        v-show="page > 0"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="size-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M15.75 19.5 8.25 12l7.5-7.5"
          />
        </svg>
      </button>

      <!-- Right button -->
      <button
        class="pagination_btn pagination_btn--right"
        @click="changePage(page + 1)"
        v-show="
          (page + 1) * PRODUCT_COUNT < productsArray[props.category].length
        "
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="size-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="m8.25 4.5 7.5 7.5-7.5 7.5"
          />
        </svg>
      </button>
    </div>

    <!-- FOOTER -->
    <footer class="main_selection__footer">
      Labloszko S.A. Wszystkie prawa zastrzeżone&copy; {{ date }}
    </footer>
  </div>
</template>

<style scoped lang="scss">
.main_selection {
  background-color: var(--color-secondary);
  width: 100%;
  display: flex;
  flex-direction: column;

  &__wrapper {
    height: 100%;
    width: 100%;
    &--outer {
      position: relative;
    }

    &--inner {
      transition: all 0.2s;
      &--animation {
        opacity: 0;
      }
    }
  }

  &__grid {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    padding: 2.4rem;
    gap: 2.4rem;

    justify-items: center;
  }

  &__product {
    padding: 1.2rem 1.2rem;
    box-shadow: 0 0.5rem 0.5rem rgba(0, 0, 0, 0.07);
    cursor: pointer;
    border-radius: var(--default-boder-radius);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.4rem;
    background-color: var(--color-secondary-light);
    transition: 0.2s all;

    &:hover {
      transform: translateY(-4px);
      box-shadow: 0 1rem 1rem rgba(0, 0, 0, 0.1);
    }

    &:active {
      transform: translateY(2px);

      box-shadow: 0 1rem 1rem rgba(0, 0, 0, 0.05);
    }

    &__image {
      height: 18rem;
      width: 18rem;
      border-radius: var(--default-boder-radius);
    }

    &__name {
      margin-top: 0.8rem;
      font-size: 1.6rem;
      color: var(--color-primary-dark);
      text-transform: uppercase;
    }

    &__price {
      font-size: 1.2rem;
      color: var(--color-primary);
    }
  }

  &__footer {
    text-transform: uppercase;
    color: var(--color-primary);
    font-size: 1.2rem;
    text-align: center;
    padding: 1.2rem 2.4rem;
  }
}

.pagination {
  margin-top: auto;

  display: flex;
  padding: 2.4rem;
  width: 100%;

  &_btn {
    background-color: transparent;
    border: solid 1px var(--color-primary);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    height: 4.2rem;
    width: 4.2rem;
    transition: all 0.2s;

    &:hover {
      background-color: var(--color-primary);

      svg {
        stroke: var(--color-secondary);
      }
    }

    svg {
      height: 3.6rem;
      width: 3.6rem;
      stroke: var(--color-primary);
      stroke-width: 1px;
      transition: all 0.2s;
    }

    &--right {
      margin-left: auto;
    }

    &--left {
      margin-right: auto;
    }
  }
}

.overlay {
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  filter: blur(5px);
}
</style>
