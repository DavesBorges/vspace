<template>
  <header>
    <div class="wrapper">
      <nav>
        <ControllerComponent @change="value = $event" />
      </nav>
    </div>
  </header>

  <CanvasComponent :data="data" />
</template>

<script lang="ts" setup>
import CanvasComponent from './components/CanvasComponent.vue';
import ControllerComponent from './components/ControllerComponent.vue';
import { ref, unref, watchEffect } from 'vue';

const value = ref({ x: 0, y: 0 });

const data = ref<[number, number][]>([[unref(value.value.x), unref(value.value.y)]]);
watchEffect(() => (data.value = [[unref(value.value.x), unref(value.value.y)]]));
</script>

<style scoped>
nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
