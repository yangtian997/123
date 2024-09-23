<script setup lang="ts">
const router = useRouter()
const goHome = () => {
  router.push('/')
}

const props = defineProps({
  keyword: {
    type: String,
    default: () => ''
  }
})
const searchKeyword = ref(props.keyword)
const emit = defineEmits(['search'])
const search = () => {
  emit('search',searchKeyword.value)
}
const goGithub = () => {
  window.open('https://github.com/unilei/aipan-netdisk-search.git')
}
const colorMode = useColorMode()

console.log(colorMode.preference)

</script>

<template>
  <el-affix>
    <div class="bg-white dark:bg-gray-800 shadow px-[10px] md:px-[20px] py-[10px]">
      <div class="max-w-[1240px] mx-auto h-[40px]  flex flex-row items-center gap-2 md:gap-6 relative">

        <div class="flex flex-row items-center gap-1">
          <img class="w-[30px] h-[30px] md:w-[40px] md:h-[40px] cursor-pointer" src="https://tc.qianying888.com/app/hide.php?key=cEpMNWhiZEdSRXhvM2lrWEV0aTNBV24wTk1rWVNvbE4=" alt="logo" @click="goHome()">
          <h1 class="hidden md:block text-[14px] font-serif font-bold cursor-pointer dark:text-white" @click="goHome()" >灵梦-网盘资源搜索</h1>
        </div>

        <div class="w-[220px] md:w-[400px]">
          <client-only>
            <el-input class="h-[30px]"
                      v-model="searchKeyword"
                      placeholder="请输入关键词搜索"
                      @keydown.enter="search()"
                      prefix-icon="Search"
                      size="small"
                      clearable
            >
            </el-input>
          </client-only>
        </div>

        

      </div>
    </div>
  </el-affix>
</template>

<style scoped>
:deep(.el-input__wrapper.is-focus) {
  --el-input-focus-border-color: #6648ff;
}
</style>
