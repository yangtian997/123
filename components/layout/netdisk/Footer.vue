<script setup>

const router = useRouter()
const donate = () => {
    router.push({ path: '/donate' })
}

</script>
<template>
    <div class="py-2 h-25 fixed bottom-0 left-0 right-0 bg-white/70  backdrop-blur dark:bg-slate-800 space-y-1">
        <div class="flex flex-row items-center justify-center  gap-3">
            <a class="" href="https://zr.qianying888.com/">
                <img class="w-5 h-5" src="https://tc.qianying888.com/app/hide.php?key=WFBRYUNPOTFlRGtkSW1PVDdLa2N2SGhIdHNwYWxQamI=" alt="去水印">
            </a>
        </div>
        <p class="text-center text-[8px] sm:text-[12px] text-slate-400">
            声明：本站内容皆来自网络公开资源。本站不储存、复制、传播任何文件，不做任何盈利，仅作个人公益学习，请勿非法&商业传播，如有侵权，请及时
            <a class="text-blue-500 underline" href="mailto:362856178@qq.com" title="点击留言"> 留言 </a> 告知删除。
        </p>
    </div>
</template>
<style scoped></style>
