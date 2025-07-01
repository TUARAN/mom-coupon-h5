<template>
  <div class="coupon-card bg-white/95 backdrop-blur-sm rounded-3xl shadow-2xl p-6 transform transition-all duration-500 hover:scale-105">
    <!-- 商品图片占位符 -->
    <div class="relative mb-6">
      <div class="w-full h-48 bg-gradient-to-br from-pink-100 to-blue-100 rounded-2xl flex items-center justify-center overflow-hidden">
        <div class="text-center">
          <div class="text-6xl mb-2">{{ getEmoji(coupon.category) }}</div>
          <div class="text-sm text-gray-600">{{ coupon.category }}</div>
        </div>
      </div>
      <!-- 优惠标签 -->
      <div class="absolute -top-2 -right-2 bg-red-500 text-white text-xs px-3 py-1 rounded-full font-bold animate-pulse">
        限时特惠
      </div>
    </div>

    <!-- 商品信息 -->
    <div class="mb-4">
      <h3 class="text-xl font-bold text-gray-800 mb-2 line-clamp-2">{{ coupon.title }}</h3>
      <div class="flex items-center gap-2 mb-3">
        <span class="text-xs bg-pink-100 text-pink-600 px-3 py-1 rounded-full font-medium">
          {{ coupon.category }}
        </span>
      </div>
    </div>

    <!-- 价格区域 -->
    <div class="mb-4">
      <div class="flex items-baseline gap-3 mb-2">
        <span class="text-3xl font-bold text-red-500">¥{{ coupon.price }}</span>
        <span class="text-lg text-gray-400 line-through">¥{{ coupon.originalPrice }}</span>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-sm bg-red-100 text-red-600 px-3 py-1 rounded-full font-medium">
          省¥{{ coupon.savings }}
        </span>
        <span class="text-xs text-gray-500">限时抢购</span>
      </div>
    </div>

    <!-- 优惠描述 -->
    <div class="bg-gradient-to-r from-blue-50 to-purple-50 rounded-2xl p-4 mb-6 border border-blue-100">
      <div class="flex items-start gap-2">
        <span class="text-blue-500 text-lg">💡</span>
        <p class="text-sm text-blue-800 font-medium leading-relaxed">{{ coupon.detail }}</p>
      </div>
    </div>

    <!-- 操作按钮 -->
    <div class="space-y-3">
      <!-- 主要抢购按钮 -->
      <button 
        @click="handleClick"
        class="w-full bg-gradient-to-r from-pink-500 to-red-500 text-white font-bold py-4 px-6 rounded-2xl text-lg shadow-lg hover:shadow-xl transform transition-all duration-200 hover:scale-105 active:scale-95 relative overflow-hidden group"
      >
        <span class="relative z-10 flex items-center justify-center gap-2">
          <span class="text-xl">🛒</span>
          <span>立即抢购</span>
        </span>
        <div class="absolute inset-0 bg-gradient-to-r from-pink-600 to-red-600 opacity-0 group-hover:opacity-100 transition-opacity duration-200"></div>
      </button>

      <!-- 次要操作 -->
      <div class="flex gap-3">
        <button 
          @click="handleNext"
          class="flex-1 bg-white border-2 border-gray-200 text-gray-700 font-medium py-3 px-4 rounded-xl hover:bg-gray-50 transition-colors duration-200"
        >
          下一个
        </button>
        <button 
          @click="handleShare"
          class="flex-1 bg-white border-2 border-gray-200 text-gray-700 font-medium py-3 px-4 rounded-xl hover:bg-gray-50 transition-colors duration-200"
        >
          分享
        </button>
      </div>
    </div>

    <!-- 底部提示 -->
    <div class="mt-4 text-center">
      <p class="text-xs text-gray-500">
        👆 点击抢购直接跳转购买
      </p>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  coupon: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits(['next'])

const handleClick = () => {
  // 在新窗口打开链接
  window.open(props.coupon.link, '_blank')
}

const handleNext = () => {
  emit('next')
}

const handleShare = () => {
  // 分享功能
  if (navigator.share) {
    navigator.share({
      title: props.coupon.title,
      text: `${props.coupon.title} - 限时特价¥${props.coupon.price}`,
      url: props.coupon.link
    })
  } else {
    // 复制链接到剪贴板
    navigator.clipboard.writeText(props.coupon.link).then(() => {
      alert('链接已复制到剪贴板！')
    })
  }
}

const getEmoji = (category) => {
  const emojiMap = {
    '纸尿裤': '👶',
    '奶粉': '🍼',
    '辅食': '🥣',
    '奶瓶': '🍶',
    '玩具': '🧸',
    '服装': '👕',
    '洗护': '🧴',
    '其他': '🎁'
  }
  return emojiMap[category] || '🎁'
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
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
}

.coupon-card:hover {
  border-color: rgba(255, 255, 255, 0.4);
  box-shadow: 0 35px 60px -12px rgba(0, 0, 0, 0.35);
}

/* 动画效果 */
@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}

.coupon-card {
  animation: float 6s ease-in-out infinite;
  animation-delay: calc(var(--index, 0) * 0.5s);
}

/* 按钮点击效果 */
button:active {
  transform: scale(0.95);
}
</style> 