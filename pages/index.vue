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
  <div class="bg-[#ffffff] dark:bg-gray-800  min-h-screen py-[60px]">
    <div class="flex flex-row items-center justify-center gap-3 mt-[80px]">
      <img class="w-[40px] h-[40px] sm:w-[60px] sm:h-[60px]" src="https://tc.qianying888.com/app/hide.php?key=cEpMNWhiZEdSRXhvM2lrWEV0aTNBV24wTk1rWVNvbE4=" alt="logo">
      <h1 class="text-[18px] sm:text-[22px] font-serif font-bold dark:text-white ">灵梦-最强网盘资源搜索引擎</h1>
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
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-[20px]">
        <div class="bg-white shadow-lg rounded-[5px] p-6 text-center">
          <h2 class="text-[22px] font-bold" style="color: #09afe899;">欢迎来到</h2>
          <p class="mt-4 text-[16px] font-bold">在这里没有你搜索不到的资源</p>
          <p class="mt-2 text-[16px] font-bold">禁止黄！堵！毒！</p>
        </div>
        <div class="bg-white shadow-lg rounded-[5px] p-6 text-center">
          <h2 class="text-[22px] font-bold" style="color: #09afe899;">资源反馈</h2>
          <p class="mt-4 text-[16px] font-bold">如果遇到没有的资源请联系QQ：362856178</p>
        </div>
        <div class="bg-white shadow-lg rounded-[5px] p-6 text-center">
          <h2 class="text-[22px] font-bold" style="color: #09afe899;">告知</h2>
          <p class="mt-4 text-[16px] font-bold">全部网盘资源均来自网络</p>
          <p class="mt-2 text-[16px] font-bold">如侵权请告知联系QQ删除</p>
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
.bg-white {
  background-color: #ffffff;
}
.shadow-lg {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.1);
}
</style>
