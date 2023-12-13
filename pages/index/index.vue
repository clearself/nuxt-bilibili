<template>
    <!-- 公共头部 -->
    <AppHeader />
    <!-- 频道模块 -->
    <van-tabs>
        <van-tab v-for="(item, index) in channelList" :key="index" :title="item.name" />
    </van-tabs>
    <!-- 视频列表 -->
    <van-list v-model:loading="loading" :finished="finished" finished-text="没有更多了" @load="onLoad">
        <div class="video-list">
            <AppVideo v-for="item in list" :key="item.bvid" :video-info="item" />
        </div>
    </van-list>
</template>
<script setup lang="ts">
const list = ref([]);
const loading = ref(false);
const finished = ref(false);
let page = 1,pageSize = 20;
// 获取频道列表
const { data: channelList } = await useFetch('/api/channel')
console.log(channelList.value, 'channelList')
const { data: videoList } = await useFetch('/api/video')
console.log(videoList.value, 'videoList')
const onLoad = () => {
    // 异步更新数据
    // setTimeout 仅做示例，真实场景中一般为 ajax 请求
        let result = videoList.value?.slice((page-1)*pageSize, page*pageSize) as any[];
        console.log(page,'页数')
        list.value.push(...result)
        // 加载状态结束
        loading.value = false;

        // 数据全部加载完成
        if (list.value.length === videoList.value.length) {
            finished.value = true;
        }
        page++
};
onLoad()
</script>
<style lang="scss">
.app-header {
    display: flex;
    align-items: center;
    padding: 8px 12px;
    background-color: #fff;

    .logo {
        flex: 1;

        .Navbar_logo {
            color: #fb7299;
            font-size: 28px;
        }
    }

    .search {
        padding: 0 8px;

        .ic_search_tab {
            color: #ccc;
            font-size: 22px;
        }
    }

    .face {
        padding: 0 15px;

        img {
            width: 24px;
            height: 24px;
        }
    }

    .down-app {
        font-size: 12px;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #fb7299;
        color: #fff;
        border-radius: 5px;
        padding: 5px 10px;
    }
}

// 视频列表
.video-list {
    display: flex;
    flex-wrap: wrap;
    padding: 10px 5px;
}

// 视频卡片
.v-card {
    width: 50%;
    padding: 0 5px 10px;

    .card {
        position: relative;
        background: #f3f3f3 url(@/assets/images/default.png) center no-repeat;
        background-size: 36%;
        border-radius: 2px;
        overflow: hidden;

        .card-img {
            .pic {
                height: 100px;
                width: 100%;
                object-fit: cover;
            }
        }

        .count {
            background-image: linear-gradient(0deg, #000000d9, #0000);
            color: #fff;
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 5px 6px;

            span {
                .iconfont {
                    font-size: 12px;
                }
            }
        }
    }

    .title {
        margin-top: 5px;
        font-size: 12px;
        color: #212121;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
    }
}
</style> 
