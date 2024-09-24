<script setup>
import { useDoubanStore } from "~/stores/douban";
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
  // window.open(movie.url, '_blank')
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
      <h1 class="title-text">灵梦-最强网盘资源搜索引擎</h1>
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
    <div class="card-container max-w-[1240px] mx-auto mt-[40px] flex flex-col md:flex-row justify-center items-center gap-6">
      <div class="card">
        <h2>欢迎来到</h2>
        <p>在这里没有你搜索不到的资源</p>
        <p>禁止黄！赌！毒！</p>
      </div>
      <div class="card">
        <h2>资源反馈</h2>
        <p>如果遇到没有的资源请联系QQ：362856178</p>
      </div>
      <div class="card">
        <h2>告知</h2>
        <p>全部网盘资源均来自网络</p>
        <p>如侵权请告知联系QQ删除</p>
      </div>
    </div>
    <div class="hidden-section mx-5 xl:max-w-[1200px] xl:mx-auto mt-12 mb-[100px]" v-if="doubanData.length > 0">
      <h1 class="text-[12px] sm:text-sm text-slate-600 font-bold dark:text-white mt-[20px]">热门榜单</h1>
      <div class="grid grid-cols-2 xs:grid-cols-3 md:grid-cols-5 lg:grid-cols-5 xl:grid-cols-8  gap-3  mt-[10px]">
        <div
          class="mx-1 cursor-pointer truncate text-xs font-bold dark:bg-slate-700 dark:text-slate-100 rounded-[5px] p-2 transition-transform duration-300 hover:scale-105"
          v-for="(movie, index) in doubanData" :key="index" type="info" @click="goDouban(movie)">
          <img class="w-full h-[180px] lg:h-[220px] xl:h-[161px] rounded-[5px] object-cover"
            :src="'https://images.weserv.nl/?url=' + movie.cover" alt="" referrerpolicy="never">
          <p class="mt-1  text-center truncate">
            {{ movie.title }}
            {{ movie.rate }}
          </p>
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
.hidden-section {
  display: none;
}
.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1.5rem;
}
.card {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  text-align: center;
  width: 280px;
  transition: transform 0.3s;
}
.card:hover {
  transform: translateY(-10px);
}
.card h2 {
  font-size: 1.1rem;
  color: #09afe899;
  font-weight: bold;
  text-shadow: 1px 1px 2px #fff;
}
.card p {
  font-size: 0.85rem;
  color: #333;
  font-weight: bold;
  margin-top: 10px;
}
.title-text {
  font-size: 22px;
  font-weight: bold;
  color: #ffffff;
  text-shadow: 2px 2px 0 #000,0px 0px 0 #000,2px -1px 0 #000;
}

  
</style>
