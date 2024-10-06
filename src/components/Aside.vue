<script setup>
import { ref } from "vue";
const emit = defineEmits(["selectedCategory"]);
const selections = ref(["owoce", "warzywa", "orzechy"]);
const selected = ref(selections.value[0]);
const weight = ref(0.23);

const changeSelection = function (newSelection) {
  selected.value = newSelection;
  emit("selectedCategory", selected.value);
};
</script>

<template>
  <aside class="main_aside">
    <h3 class="main_aside__header">Wybierz kategorię</h3>
    <div class="main_aside__selection__box">
      <button
        :class="{
          main_aside__selection__btn: true,
          'main_aside__selection__btn--selected': selected === value,
        }"
        v-for="value in selections"
        @click="changeSelection(value)"
      >
        {{ value }}
      </button>
    </div>
    <span class="main_aside__weight">{{ weight }}kg</span>
  </aside>
</template>

<style scoped lang="scss">
.main_aside {
  background-color: var(--color-primary);
  padding: 2.4rem 2.4rem;
  height: 100%;
  display: flex;
  flex-direction: column;
  &__header {
    text-transform: uppercase;
    text-align: center;
    font-size: 1.6rem;
    color: var(--color-secondary-light);
    margin-bottom: 2.4rem;
    text-wrap: nowrap;
  }

  &__selection {
    &__box {
      display: flex;
      flex-direction: column;

      gap: 1.2rem;
    }
    &__btn {
      background-color: var(--color-primary-light);
      color: var(--color-secondary-light);
      font-size: 1.4rem;
      text-transform: uppercase;
      border: none;
      padding: 1.6rem 3.2rem;
      transition: all 0.2s;
      box-shadow: 0 0.5rem 0.5rem rgba(0, 0, 0, 0.2);
      cursor: pointer;
      &:hover {
        background-color: var(--color-primary-dark);
      }

      &--selected {
        background-color: var(--color-secondary) !important;
        color: var(--color-primary-dark) !important;
      }
    }
  }

  &__weight {
    display: block;
    font-size: 3.6rem;
    color: var(--color-secondary-light);
    text-align: center;
    margin-top: auto;
    font-weight: 300;
  }
}
</style>
