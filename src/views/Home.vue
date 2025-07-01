<template>
  <div class="h-screen bg-black overflow-hidden">
    <!-- 顶部状态栏 -->
    <div class="fixed top-0 left-0 right-0 z-20 bg-gradient-to-b from-black/50 to-transparent h-16 flex items-center justify-center">
      <div class="text-center">
        <h1 class="text-white text-lg font-bold">👶 宝妈省钱神器</h1>
        <p class="text-white/70 text-xs">精选母婴好物，省钱更省心</p>
      </div>
    </div>

    <!-- 搜索栏 -->
    <div class="fixed top-16 left-0 right-0 z-20 bg-gradient-to-b from-black/30 to-transparent h-20 flex items-center justify-center px-4">
      <div class="w-full max-w-sm">
        <!-- 搜索输入框 -->
        <div class="relative">
          <input
            v-model="searchQuery"
            @input="handleSearch"
            @focus="showSearchResults = true"
            @blur="handleBlur"
            type="text"
            placeholder="搜索商品名称或分类..."
            class="w-full bg-white/90 backdrop-blur-sm rounded-2xl px-4 py-3 pl-12 text-gray-800 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-pink-500 focus:bg-white"
          />
          <div class="absolute left-4 top-1/2 transform -translate-y-1/2 text-gray-400">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
            </svg>
          </div>
          <!-- 清除按钮 -->
          <button
            v-if="searchQuery"
            @click="clearSearch"
            class="absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
          </button>
        </div>

        <!-- 搜索建议 -->
        <div v-if="showSearchResults && searchSuggestions.length > 0" class="mt-2 bg-white/95 backdrop-blur-sm rounded-2xl shadow-lg max-h-48 overflow-y-auto">
          <div
            v-for="suggestion in searchSuggestions"
            :key="suggestion.id"
            @click="selectSuggestion(suggestion)"
            class="px-4 py-3 hover:bg-gray-50 cursor-pointer border-b border-gray-100 last:border-b-0"
          >
            <div class="flex items-center gap-3">
              <span class="text-lg">{{ getEmoji(suggestion.category) }}</span>
              <div class="flex-1">
                <div class="text-sm font-medium text-gray-800">{{ suggestion.title }}</div>
                <div class="text-xs text-gray-500">{{ suggestion.category }} · ¥{{ suggestion.price }}</div>
              </div>
            </div>
          </div>
        </div>

        <!-- 分类筛选 -->
        <div class="mt-3 flex gap-2 overflow-x-auto pb-2">
          <button
            v-for="category in categories"
            :key="category"
            @click="filterByCategory(category)"
            class="px-4 py-2 bg-white/20 backdrop-blur-sm rounded-full text-white text-sm whitespace-nowrap transition-all duration-200"
            :class="selectedCategory === category ? 'bg-pink-500 text-white' : 'hover:bg-white/30'"
          >
            {{ getEmoji(category) }} {{ category }}
          </button>
        </div>
      </div>
    </div>

    <!-- 搜索栏 -->
    <div class="fixed top-16 left-0 right-0 z-20 bg-gradient-to-b from-black/30 to-transparent h-20 flex items-center justify-center px-4">
      <div class="w-full max-w-sm">
        <!-- 搜索输入框 -->
        <div class="relative">
          <input
            v-model="searchQuery"
            @input="handleSearch"
            @focus="showSearchResults = true"
            @blur="handleBlur"
            type="text"
            placeholder="搜索商品名称或分类..."
            class="w-full bg-white/90 backdrop-blur-sm rounded-2xl px-4 py-3 pl-12 text-gray-800 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-pink-500 focus:bg-white"
          />
          <div class="absolute left-4 top-1/2 transform -translate-y-1/2 text-gray-400">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
            </svg>
          </div>
          <!-- 清除按钮 -->
          <button
            v-if="searchQuery"
            @click="clearSearch"
            class="absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
          </button>
        </div>

        <!-- 搜索建议 -->
        <div v-if="showSearchResults && searchSuggestions.length > 0" class="mt-2 bg-white/95 backdrop-blur-sm rounded-2xl shadow-lg max-h-48 overflow-y-auto">
          <div
            v-for="suggestion in searchSuggestions"
            :key="suggestion.id"
            @click="selectSuggestion(suggestion)"
            class="px-4 py-3 hover:bg-gray-50 cursor-pointer border-b border-gray-100 last:border-b-0"
          >
            <div class="flex items-center gap-3">
              <span class="text-lg">{{ getEmoji(suggestion.category) }}</span>
              <div class="flex-1">
                <div class="text-sm font-medium text-gray-800">{{ suggestion.title }}</div>
                <div class="text-xs text-gray-500">{{ suggestion.category }} · ¥{{ suggestion.price }}</div>
              </div>
            </div>
          </div>
        </div>

        <!-- 分类筛选 -->
        <div class="mt-3 flex gap-2 overflow-x-auto pb-2">
          <button
            v-for="category in categories"
            :key="category"
            @click="filterByCategory(category)"
            class="px-4 py-2 bg-white/20 backdrop-blur-sm rounded-full text-white text-sm whitespace-nowrap transition-all duration-200"
            :class="selectedCategory === category ? 'bg-pink-500 text-white' : 'hover:bg-white/30'"
          >
            {{ getEmoji(category) }} {{ category }}
          </button>
        </div>
      </div>
    </div>

    <!-- 优惠券滑动容器 -->
    <div 
      class="h-full overflow-y-auto snap-y snap-mandatory pt-36" 
      ref="scrollContainer"
      @touchstart="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
    >
      <div class="space-y-0">
        <div 
          v-for="(coupon, index) in filteredCoupons" 
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
              v-for="(item, i) in filteredCoupons" 
              :key="i"
              class="w-2 h-2 rounded-full transition-all duration-300"
              :class="i === index ? 'bg-white' : 'bg-white/30'"
            ></div>
          </div>

          <!-- 当前商品序号 -->
          <div class="absolute top-20 left-4 text-white/60 text-sm">
            {{ index + 1 }} / {{ filteredCoupons.length }}
          </div>
        </div>
      </div>

      <!-- 无搜索结果 -->
      <div v-if="filteredCoupons.length === 0" class="h-screen flex items-center justify-center">
        <div class="text-center text-white">
          <div class="text-6xl mb-4">🔍</div>
          <h3 class="text-xl font-bold mb-2">没有找到相关商品</h3>
          <p class="text-white/70 mb-4">试试其他关键词或分类</p>
          <button
            @click="clearSearch"
            class="bg-white/20 backdrop-blur-sm text-white px-6 py-3 rounded-2xl hover:bg-white/30 transition-colors"
          >
            清除搜索
          </button>
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
import { ref, onMounted, onUnmounted, computed } from 'vue'
import CouponCard from '../components/CouponCard.vue'
import couponsData from '../data/coupons.json'

const coupons = ref([])
const loading = ref(true)
const scrollContainer = ref(null)

// 搜索相关变量
const searchQuery = ref('')
const showSearchResults = ref(false)
const selectedCategory = ref('')
const searchSuggestions = ref([])

// 触摸相关变量
const touchStartY = ref(0)
const touchEndY = ref(0)
const currentIndex = ref(0)

// 获取所有分类
const categories = computed(() => {
  const cats = [...new Set(coupons.value.map(item => item.category))]
  return cats
})

// 过滤后的商品列表
const filteredCoupons = computed(() => {
  let filtered = coupons.value

  // 按分类筛选
  if (selectedCategory.value) {
    filtered = filtered.filter(item => item.category === selectedCategory.value)
  }

  // 按搜索关键词筛选
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.filter(item => 
      item.title.toLowerCase().includes(query) ||
      item.category.toLowerCase().includes(query) ||
      item.detail.toLowerCase().includes(query)
    )
  }

  return filtered
})

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

// 搜索处理
const handleSearch = () => {
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase()
    searchSuggestions.value = coupons.value.filter(item => 
      item.title.toLowerCase().includes(query) ||
      item.category.toLowerCase().includes(query)
    ).slice(0, 5) // 最多显示5个建议
  } else {
    searchSuggestions.value = []
  }
}

// 选择搜索建议
const selectSuggestion = (suggestion) => {
  searchQuery.value = suggestion.title
  showSearchResults.value = false
  // 滚动到对应商品
  const index = filteredCoupons.value.findIndex(item => item.id === suggestion.id)
  if (index !== -1) {
    scrollToIndex(index)
  }
}

// 按分类筛选
const filterByCategory = (category) => {
  if (selectedCategory.value === category) {
    selectedCategory.value = ''
  } else {
    selectedCategory.value = category
  }
  searchQuery.value = ''
  showSearchResults.value = false
}

// 清除搜索
const clearSearch = () => {
  searchQuery.value = ''
  selectedCategory.value = ''
  searchSuggestions.value = []
  showSearchResults.value = false
}

// 处理失焦
const handleBlur = () => {
  setTimeout(() => {
    showSearchResults.value = false
  }, 200)
}

// 获取emoji
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

// 滚动到指定索引
const scrollToIndex = (index) => {
  if (scrollContainer.value) {
    const scrollTop = index * window.innerHeight
    scrollContainer.value.scrollTo({
      top: scrollTop,
      behavior: 'smooth'
    })
  }
}

// 滚动到下一个
const scrollToNext = () => {
  if (scrollContainer.value && currentIndex.value < filteredCoupons.value.length - 1) {
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

/* 搜索建议滚动条 */
.overflow-y-auto::-webkit-scrollbar {
  width: 4px;
}

.overflow-y-auto::-webkit-scrollbar-track {
  background: transparent;
}

.overflow-y-auto::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 2px;
}
</style> 