<script setup lang="ts">
import { ref, onMounted, watch, nextTick } from 'vue'

const props = defineProps<{
  initialFontSize?: number
  text: string
  minFontSize?: number
}>()

const container = ref<HTMLElement | null>(null)
const content = ref<HTMLElement | null>(null)
const currentFontSize = ref(props.initialFontSize || 16)
const isEllipsis = ref(false)

const adjustFontSize = async () => {
  if (!container.value || !content.value) return

  // 重置字体大小和省略号状态
  currentFontSize.value = props.initialFontSize || 16
  isEllipsis.value = false
  content.value.style.textOverflow = 'clip'
  
  await nextTick()

  const minSize = props.minFontSize || 12
  
  while (
    (content.value.scrollWidth > container.value.clientWidth ||
     content.value.scrollHeight > container.value.clientHeight) &&
    currentFontSize.value > minSize
  ) {
    currentFontSize.value--
    await nextTick()
  }

  // 如果字体已经最小但还是溢出，则使用省略号
  if (
    currentFontSize.value <= minSize &&
    (content.value.scrollWidth > container.value.clientWidth ||
     content.value.scrollHeight > container.value.clientHeight)
  ) {
    isEllipsis.value = true
    content.value.style.textOverflow = 'ellipsis'
  }
}

watch(() => props.text, () => {
  nextTick(adjustFontSize)
})

onMounted(() => {
  adjustFontSize()
})
</script>

<template>
  <div 
    ref="container" 
    class="dynamic-text-container"
  >
    <div
      ref="content"
      class="dynamic-text-content"
      :style="{
        fontSize: `${currentFontSize}px`,
      }"
    >
      {{ text }}
    </div>
  </div>
</template>

<style scoped>
.dynamic-text-container {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.dynamic-text-content {
  width: 100%;
  height: 100%;
  white-space: nowrap;
  overflow: hidden;
}
</style>
