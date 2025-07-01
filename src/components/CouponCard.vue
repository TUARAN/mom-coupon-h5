<template>
  <div class="coupon-card bg-white rounded-2xl shadow-card p-4 mb-4 transform transition-all duration-300 hover:scale-105 hover:shadow-lg">
    <!-- 商品信息区域 -->
    <div class="flex items-start justify-between mb-3">
      <div class="flex-1">
        <h3 class="text-lg font-semibold text-gray-800 mb-1 line-clamp-2">{{ coupon.title }}</h3>
        <div class="flex items-center gap-2 mb-2">
          <span class="text-xs bg-pink-light text-pink-600 px-2 py-1 rounded-full font-medium">
            {{ coupon.category }}
          </span>
        </div>
      </div>
    </div>

    <!-- 价格区域 -->
    <div class="flex items-center gap-2 mb-3">
      <span class="text-2xl font-bold text-red-500">¥{{ coupon.price }}</span>
      <span class="text-sm text-gray-500 line-through">¥{{ coupon.originalPrice }}</span>
      <span class="text-xs bg-red-100 text-red-600 px-2 py-1 rounded-full font-medium">
        省¥{{ coupon.savings }}
      </span>
    </div>

    <!-- 优惠描述 -->
    <div class="bg-blue-soft rounded-xl p-3 mb-4">
      <p class="text-sm text-blue-800 font-medium">{{ coupon.detail }}</p>
    </div>

    <!-- 跳转按钮 -->
    <button 
      @click="handleClick"
      class="w-full bg-gradient-to-r from-pink-warm to-pink-500 text-white font-semibold py-4 px-6 rounded-xl text-lg shadow-soft hover:shadow-lg transform transition-all duration-200 hover:scale-105 active:scale-95"
    >
      👉 去抢购
    </button>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  coupon: {
    type: Object,
    required: true
  }
})

const handleClick = () => {
  // 在新窗口打开链接
  window.open(props.coupon.link, '_blank')
}
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.coupon-card {
  border: 1px solid #f3f4f6;
}

.coupon-card:hover {
  border-color: #fbbf24;
}
</style> 