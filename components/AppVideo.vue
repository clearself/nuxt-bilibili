<template>
    <NuxtLink class="v-card" :to="`/video/${videoInfo.bvid}`">
        <div class="card">
            <div class="card-img">
                <img class="pic" :src="videoInfo.pic" :alt="videoInfo.title" />
            </div>
            <div class="count">
                <span>
                    <i class="iconfont icon_shipin_bofangshu"></i>
                    {{ videoInfo.stat.view }}
                </span>
                <span>
                    <i class="iconfont icon_shipin_danmushu"></i>
                    {{ videoInfo.stat.danmaku }}
                </span>
            </div>
        </div>
        <p class="title">{{ videoInfo.title }}</p>
    </NuxtLink>
</template>
<script setup>
defineProps({
    videoInfo:{
        type:Object,
        default:()=>{}
    }
})
</script>