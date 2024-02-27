<script setup lang="ts">
import CharMain from './components/CharMain.vue';
import FeaturesSection from './components/FeaturesSection.vue';
</script>

<template>
  <header>
    <div class="wrapper">
      <i class="gg-template"></i>
      <h1>RPD</h1> <span class="sub">- Role Playing Database</span>
    </div>
  </header>

  <main>
    <FeaturesSection />
    <CharMain />
  </main>

  <footer>
    <div>
      <p>&#169; 2024 Øyvind Langeggen</p>
    </div>
  </footer>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

header {
    display: flex;
    place-items: center;
    padding-top: 10px;
  }
  h1 {
    padding-top: 7px;
    font-size: 34px;
  }
  .sub {
    padding-top: 7px;
    font-size: 12px;
    margin-left: -5px;
  }
  .logo {
    margin: 0 2rem 0 0;
  }
  header .wrapper {
    display: flex;
    align-items: center;
    gap: 14px;
    width: 100%;
    justify-content: flex-start;
  }
  main {
    padding-top: 30px;
  }
  footer {
    margin-top: 50px;
  }

@media (min-width: 1024px) {
  header .wrapper {
    justify-content: center;
  }
}
</style>
