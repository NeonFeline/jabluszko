<script setup>
import { ref } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
import Navigation from "./components/Navigation.vue";
import Aside from "./components/Aside.vue";
import Selection from "./components/Selection.vue";

const category = ref("owoce");
const currentProduct = ref({});
const overlayActive = ref(false);

const getCategory = function (e) {
  category.value = e;
};

const printLabel = function () {
  console.log(currentProduct.value.name);
  console.log(currentProduct.value.price);
  console.log("Label printed");
};

const btnAnsYes = function () {
  printLabel();
  overlayActive.value = false;
};
const btnAnsNo = function () {
  overlayActive.value = false;
};

const getSeletedProduct = function (product) {
  currentProduct.value = product;
  overlayActive.value = true;
};
</script>

<template>
  <div :class="{ all: true, overlay: overlayActive }">
    <Navigation />

    <article class="content">
      <Aside @selectedCategory="getCategory" />
      <Suspense>
        <!-- component with nested async dependencies -->
        <Selection :category="category" @selectedProduct="getSeletedProduct" />

        <!-- loading state via #fallback slot -->
        <template #fallback>
          Loading...
        </template>
      </Suspense>
    </article>
  </div>
  <div
    :class="{ overlay__box: true, hidden: !overlayActive }"
    @click="overlayActive = !overlayActive"
  >
    <div class="overlay__innerbox" @click.stop="">
      <span class="overlay__question">Czy chcesz wydrukować etykietę?</span>
      <div class="overlay__question__box">
        <button class="overlay__question__box__btn" @click="btnAnsNo">
          Nie
        </button>
        <button
          class="overlay__question__box__btn overlay__question__box__btn--yes"
          @click="btnAnsYes"
        >
          Tak
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.all {
  height: 100vh;
  display: flex;
  flex-direction: column;
  transition: all 0.5s;
}

.content {
  display: flex;
  flex: 1;
}
.overlay {
  filter: blur(5px) brightness(0.8);
  transform: scale(1.02);
}

.overlay__innerbox {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 6.4rem 12.8rem;
  background-color: var(--color-secondary-light);
}

.overlay__box {
  // display: none;
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
}

.overlay__question {
  font-size: 2rem;
  color: var(--color-primary-dark);
}

.overlay__question__box {
  display: flex;
  justify-content: space-between;
  margin-top: 2.4rem;
}

.overlay__question__box__btn {
  background-color: transparent;
  border: none;
  border: 2px solid var(--color-primary);
  font-size: 2rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--color-primary);
  cursor: pointer;
  padding: 2.4rem 4.8rem;
  box-shadow: 0 1rem 1rem rgba(0, 0, 0, 0.1);
  transition: all 0.2s;

  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 1.5rem 1.5rem rgba(0, 0, 0, 0.15);
  }

  &:active {
    transform: translateY(2px);
    box-shadow: 0 0.5rem 0.5rem rgba(0, 0, 0, 0.05);
  }

  &--yes {
    background-color: var(--color-primary);
    color: var(--color-secondary-light);
  }
}

.hidden {
  display: none;
}
</style>
