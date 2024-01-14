<template>
  <div class="wrapper">
    <div class="container">
      <div class="form-group">
        <label> X: {{ xValue }}</label>
        <input step="1" max="5" min="-5" v-model="xValue" type="range" />
      </div>
      <div class="form-group">
        <label> Y: {{ yValue }}</label>
        <input step="1" max="5" min="-5" v-model="yValue" type="range" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue';

const xValue = ref(0);
const yValue = ref(0);
const emits = defineEmits<{ change: [value: { x: number; y: number }] }>();
watchEffect(() => emits('change', { x: Number(xValue.value), y: Number(yValue.value) }));
</script>

<style scoped>
.container {
  margin: 0 auto;
  width: 30%;
}
.form-group {
  max-width: 200px;
  label {
    margin-right: 10px;
  }
  display: flex;
  justify-content: space-between;
}
</style>
