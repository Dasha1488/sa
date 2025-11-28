<template>
  <div class="home-page">
    <div class="container">
      <h1>Калькулятор подсетей</h1>
      <SubnetCalculator />
    </div>
  </div>
</template>

<script setup lang="ts">
import SubnetCalculator from '@/components/SubnetCalculator.vue'
</script>

<style scoped>
.home-page {
  padding: 20px;
  min-height: 100vh;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}
</style>