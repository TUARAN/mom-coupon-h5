<template>
  <div class="min-h-screen bg-gradient-to-br from-pink-light via-white-warm to-blue-soft">
    <!-- 顶部标题区域 -->
    <header class="bg-white shadow-soft sticky top-0 z-10">
      <div class="container mx-auto px-4 py-4">
        <div class="flex items-center justify-center">
          <div class="text-center">
            <h1 class="text-2xl font-bold text-gray-800 mb-1">
              👶 宝妈省钱神器
            </h1>
            <p class="text-sm text-gray-600">精选母婴好物，省钱更省心</p>
          </div>
        </div>
      </div>
    </header>

    <!-- 主要内容区域 -->
    <main class="container mx-auto px-4 py-6">
      <!-- 今日神券合集标题 -->
      <div class="mb-6">
        <div class="flex items-center gap-3 mb-4">
          <div class="w-1 h-8 bg-gradient-to-b from-pink-warm to-pink-500 rounded-full"></div>
          <h2 class="text-xl font-bold text-gray-800">今日神券合集</h2>
        </div>
        <p class="text-sm text-gray-600 ml-4">精选8款超值母婴用品，限时优惠中</p>
      </div>

      <!-- 优惠券列表 -->
      <div class="space-y-4">
        <CouponCard 
          v-for="coupon in coupons" 
          :key="coupon.id" 
          :coupon="coupon"
        />
      </div>

      <!-- 底部提示 -->
      <div class="mt-8 text-center">
        <div class="bg-white rounded-2xl shadow-soft p-4">
          <p class="text-sm text-gray-600 mb-2">💡 温馨提示</p>
          <p class="text-xs text-gray-500">
            点击"去抢购"按钮可直接跳转到拼多多购买页面，价格以实际页面为准
          </p>
        </div>
      </div>
    </main>

    <!-- 加载状态 -->
    <div v-if="loading" class="fixed inset-0 bg-white bg-opacity-80 flex items-center justify-center z-50">
      <div class="text-center">
        <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-pink-warm mx-auto mb-4"></div>
        <p class="text-gray-600">正在加载优惠券...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import CouponCard from '../components/CouponCard.vue'
import couponsData from '../data/coupons.json'

const coupons = ref([])
const loading = ref(true)

onMounted(() => {
  // 模拟加载延迟，实际项目中可以调用API
  setTimeout(() => {
    coupons.value = couponsData
    loading.value = false
  }, 500)
})
</script>

<style scoped>
.container {
  max-width: 480px;
}

/* 移动端优化 */
@media (max-width: 480px) {
  .container {
    padding-left: 1rem;
    padding-right: 1rem;
  }
}
</style> 