<script setup lang="ts">
import { useRoute } from 'vitepress'

import { nextTick, onBeforeUnmount, onMounted, watch } from 'vue'

import { TwikooData, initTwikoo } from '../types'

const props = defineProps<{
  Twikoo_Data: TwikooData
}>()

// 组件挂载时初始化 Twikoo
onMounted(async () => {
  await nextTick() // 确保 DOM 更新完成
  await initTwikoo(props.Twikoo_Data.envId) // 直接调用 initTwikoo
})

// 在组件卸载时清理 Twikoo 评论系统
onBeforeUnmount(() => {
  const el = document.querySelector('#twikoo')
  if (el) {
    el.innerHTML = '' // 清空评论组件的内容
  }
})

// 使用 VitePress 的 useRoute 监听路由变化
const route = useRoute()

// 监听路由变化，重新加载 Twikoo 评论系统
watch(
  () => route.path,
  async () => {
    await nextTick() // 等待 DOM 更新
    setTimeout(() => initTwikoo(props.Twikoo_Data.envId), 500) // 直接调用 initTwikoo
  }
)
</script>

<template>
  <section
    id="twikoo"
    class="comment-container vp-raw"
    aria-label="用户评论区域"
  ></section>
</template>
