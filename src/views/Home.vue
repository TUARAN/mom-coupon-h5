<template>
  <div class="h-screen bg-black overflow-hidden">
    <!-- 顶部状态栏 -->
    <div class="fixed top-0 left-0 right-0 z-20 bg-gradient-to-b from-black/50 to-transparent h-16 flex items-center justify-center">
      <div class="text-center">
        <h1 class="text-white text-lg font-bold">👶 宝妈省钱神器</h1>
        <p class="text-white/70 text-xs">精选母婴好物，省钱更省心</p>
      </div>
    </div>

    <!-- 优惠券滑动容器 -->
    <div 
      class="h-full overflow-y-auto snap-y snap-mandatory" 
      ref="scrollContainer"
      @touchstart="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
    >
      <div class="space-y-0">
        <div 
          v-for="(coupon, index) in coupons" 
          :key="coupon.id"
          class="h-screen snap-start flex items-center justify-center p-4 relative"
          :style="{ background: getGradientBackground(index) }"
        >
          <!-- 商品卡片 -->
          <div class="w-full max-w-sm mx-auto">
            <CouponCard 
              :coupon="coupon" 
              :index="index"
              @next="scrollToNext"
            />
          </div>

          <!-- 滑动提示 -->
          <div class="absolute bottom-8 left-1/2 transform -translate-x-1/2 text-white/60 text-center">
            <div class="animate-bounce mb-2">
              <svg class="w-6 h-6 mx-auto" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path>
              </svg>
            </div>
            <p class="text-xs">向上滑动查看更多</p>
          </div>

          <!-- 进度指示器 -->
          <div class="absolute top-20 right-4 flex flex-col space-y-2">
            <div 
              v-for="(item, i) in coupons" 
              :key="i"
              class="w-2 h-2 rounded-full transition-all duration-300"
              :class="i === index ? 'bg-white' : 'bg-white/30'"
            ></div>
          </div>

          <!-- 当前商品序号 -->
          <div class="absolute top-20 left-4 text-white/60 text-sm">
            {{ index + 1 }} / {{ coupons.length }}
          </div>
        </div>
      </div>
    </div>

    <!-- 加载状态 -->
    <div v-if="loading" class="fixed inset-0 bg-black flex items-center justify-center z-50">
      <div class="text-center">
        <div class="animate-spin rounded-full h-16 w-16 border-b-2 border-pink-warm mx-auto mb-4"></div>
        <p class="text-white">正在加载精选好物...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import CouponCard from '../components/CouponCard.vue'
import couponsData from '../data/coupons.json'

const coupons = ref([])
const loading = ref(true)
const scrollContainer = ref(null)

// 触摸相关变量
const touchStartY = ref(0)
const touchEndY = ref(0)
const currentIndex = ref(0)

onMounted(() => {
  // 模拟加载延迟
  setTimeout(() => {
    coupons.value = couponsData
    loading.value = false
  }, 1000)

  // 监听滚动事件，更新当前索引
  if (scrollContainer.value) {
    scrollContainer.value.addEventListener('scroll', updateCurrentIndex)
  }
})

onUnmounted(() => {
  if (scrollContainer.value) {
    scrollContainer.value.removeEventListener('scroll', updateCurrentIndex)
  }
})

// 更新当前索引
const updateCurrentIndex = () => {
  if (scrollContainer.value) {
    const scrollTop = scrollContainer.value.scrollTop
    const windowHeight = window.innerHeight
    currentIndex.value = Math.round(scrollTop / windowHeight)
  }
}

// 触摸开始
const handleTouchStart = (e) => {
  touchStartY.value = e.touches[0].clientY
}

// 触摸移动
const handleTouchMove = (e) => {
  e.preventDefault()
}

// 触摸结束
const handleTouchEnd = (e) => {
  touchEndY.value = e.changedTouches[0].clientY
  const diff = touchStartY.value - touchEndY.value
  
  // 如果滑动距离足够大，则切换页面
  if (Math.abs(diff) > 50) {
    if (diff > 0) {
      // 向上滑动，显示下一个
      scrollToNext()
    } else {
      // 向下滑动，显示上一个
      scrollToPrev()
    }
  }
}

// 生成渐变背景
const getGradientBackground = (index) => {
  const gradients = [
    'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
    'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)',
    'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)',
    'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)',
    'linear-gradient(135deg, #fa709a 0%, #fee140 100%)',
    'linear-gradient(135deg, #a8edea 0%, #fed6e3 100%)',
    'linear-gradient(135deg, #ff9a9e 0%, #fecfef 100%)',
    'linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%)'
  ]
  return gradients[index % gradients.length]
}

// 滚动到下一个
const scrollToNext = () => {
  if (scrollContainer.value && currentIndex.value < coupons.value.length - 1) {
    const nextScrollTop = (currentIndex.value + 1) * window.innerHeight
    
    scrollContainer.value.scrollTo({
      top: nextScrollTop,
      behavior: 'smooth'
    })
  }
}

// 滚动到上一个
const scrollToPrev = () => {
  if (scrollContainer.value && currentIndex.value > 0) {
    const prevScrollTop = (currentIndex.value - 1) * window.innerHeight
    
    scrollContainer.value.scrollTo({
      top: prevScrollTop,
      behavior: 'smooth'
    })
  }
}
</script>

<style scoped>
/* 隐藏滚动条 */
.overflow-y-auto::-webkit-scrollbar {
  display: none;
}

.overflow-y-auto {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

/* 平滑滚动 */
.overflow-y-auto {
  scroll-behavior: smooth;
}

/* 确保每个卡片占满屏幕 */
.h-screen {
  min-height: 100vh;
}

/* 触摸优化 */
* {
  -webkit-tap-highlight-color: transparent;
  touch-action: pan-y;
}

/* 防止文本选择 */
.no-select {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style> 