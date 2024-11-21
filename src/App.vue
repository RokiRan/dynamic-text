<script setup lang="ts">
import { ref } from 'vue';
import DynamicText from './components/DynamicText.vue'

const text = ref('这是测试的文字')
</script>

<template>
  <div class="container">
    <div class="demo-box">
      <h2>Dynamic Text Demo</h2>
      <input type="text" v-model="text">
      <div class="text-container">
        <DynamicText
          :text="text"
          :initial-font-size="24"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.demo-box {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin-top: 20px;
}

.text-container {
  width: 200px;
  height: 40px;
  border: 1px solid #ccc;
  margin: 20px 0;
}

h2 {
  margin: 0;
  color: #333;
}
</style>
