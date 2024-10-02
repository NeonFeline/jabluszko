<script setup>
import { ref } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
import Navigation from "./components/Navigation.vue";
import Aside from "./components/Aside.vue";
import Selection from "./components/Selection.vue";

const category = ref("owoce");

const getCategory = function (e) {
  category.value = e;
};
</script>

<template>
  <Navigation />

  <article class="content">
    <Aside @selectedCategory="getCategory" />
    <Suspense>
      <!-- component with nested async dependencies -->
      <Selection :category="category" />

      <!-- loading state via #fallback slot -->
      <template #fallback>
        Loading...
      </template>
    </Suspense>
  </article>
</template>

<style scoped lang="scss">
.content {
  display: flex;
  flex: 1;
}
</style>
