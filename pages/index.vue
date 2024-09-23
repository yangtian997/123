<script setup>
import { useDoubanStore } from "~//douban";
import { badWords } from "~/utils/sensitiveWords";

definePageMeta({
  layout: 'netdisk',
})
const doubanStore = useDoubanStore()
const searchKeyword = ref('')
const router = useRouter()
const doubanCache = useCookie('doubanCache', {
  maxAge: 60 * 60 * 24
})

const search = (keyword) => {
  if (!keyword) return
  if (badWords.includes(keyword)) {
    return alert('请勿输入敏感词')
  }
  router.push({ path: '/search', query: { keyword: encodeURIComponent(keyword) } })
}

const doubanData = ref([])

watch(doubanData, (newValue, oldValue) => {
  doubanData.value = newValue
})

const colorMode = useColorMode()

const goDouban = (movie) => {
  router.push({ path: '/search', query: { keyword: encodeURIComponent(movie.title) } })
}

onMounted(async () => {
  if (doubanCache.value === 'exist') {
    doubanData.value = doubanStore.doubanData
  } else {
    await doubanStore.getDoubanData()
    doubanData.value = doubanStore.doubanData
    doubanCache.value = 'exist'
  }
})
</script>

<template>
  <div class="bg-[#ffffff] dark:bg-gray-800 min-h-screen py-[60px]">
    <div class="flex flex-row items-center justify-center gap-3 mt-[80px]">
      <img class="w-[40px] h-[40px] sm:w-[60px] sm:h-[60px]" src="https://tc.qianying888.com/app/hide.php?key=cEpMNWhiZEdSRXhvM2lrWEV0aTNBV24wTk1rWVNvbE4=" alt="logo">
      <h1 class="text-[18px] sm:text-[22px] font-serif font-bold dark:text-white">灵梦-最强网盘资源搜索引擎</h1>
    </div>
    <div class="max-w-[1240px] mx-auto mt-[20px]">
      <div class="w-[80%] md:w-[700px] mx-auto flex flex-row items-center gap-2 relative">
        <input class="w-full pl-6 pr-[60px] py-3 border border-gray-300 rounded-full text-sm" v-model="searchKeyword"
          placeholder="请输入关键词搜索" @keydown.enter="search(searchKeyword)" />
        <button type="button"
          class="absolute right-6 flex items-center transition-transform duration-200 hover:scale-110"
          @click="search(searchKeyword)">
          <el-icon size="20px">
            <Search></Search>
          </el-icon>
        </button>
      </div>
    </div>
    <div class="mx-5 xl:max-w-[1200px] xl:mx-auto mt-12 mb-[100px]">
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="card">
          <h2 class="card-title">欢迎来到</h2>
          <p class="card-text">在这里没有你搜索不到的资源</p>
          <p class="card-text">禁止黄！堵！毒！</p>
        </div>
        <div class="card">
          <h2 class="card-title">资源反馈</h2>
          <p class="card-text">如果遇到没有的资源请联系QQ：362856178</p>
        </div>
        <div class="card">
          <h2 class="card-title">告知</h2>
          <p class="card-text">全部网盘资源均来自网络</p>
          <p class="card-text">如侵权请告知联系QQ删除</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
:deep(.el-input__wrapper.is-focus) {
  --el-input-focus-border-color: #6648ff;
}
.border-gray-300 {
  border: 1px dashed #09afe899!important;
}
.card {
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  transition: transform 0.3s;
}
.card:hover {
  transform: translateY(-10px);
}
.card-title {
  font-size: 24px;
  font-weight: bold;
  color: #09afe8;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
}
.card-text {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-top: 10px;
}
</style>
