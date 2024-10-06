<script setup>
defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <nav class="main_nav">
    <img src="/logo.png" alt="" class="main_nav__logo" />
    <button class="main_nav__btn">Wezwij pomoc</button>
  </nav>
</template>

<style scoped lang="scss">
.main_nav {
  background-color: var(--color-primary);
  display: flex;
  padding: 1.2rem 2.4rem;
  align-items: center;
  &__logo {
    height: 3.2rem;
    margin-right: auto;
  }
  &__btn {
    background-color: var(--color-secondary);
    color: var(--color-primary-dark);
    text-transform: uppercase;
    border: none;
    padding: 1.6rem 3.2rem;
    font-size: 1.4rem;
    transition: all 0.2s;
    cursor: pointer;

    &:hover {
      background-color: var(--color-secondary-light);
    }
  }
}
</style>
