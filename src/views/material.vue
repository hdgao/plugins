<template>
    <div class="page">
        <!-- 搜索 -->
        <search @onSearch="onSearch"></search>
        <van-tabs v-model:active="active" animated>
            <van-tab v-for="item in options" :title="item.label">
                <van-pull-refresh v-model="refreshing" @refresh="onRefresh">
                    <van-list v-model:loading="loading" :finished="finished" finished-text="没有更多了" @load="onLoad">
                        <component :is="currentTabComponent" :list="list"></component>
                    </van-list>
                </van-pull-refresh>
            </van-tab>
        </van-tabs>
        <!-- 回到顶部 -->
        <ScrollTop></ScrollTop>
    </div>
</template>

<script setup lang="ts">
import { Tab as vanTab, Tabs as vanTabs, List as vanList, PullRefresh as vanPullRefresh, Icon as vanIcon } from 'vant'
import Search from '@/components/Search.vue'
import Video from '@/components/Video.vue'
import Image from '@/components/Image.vue'
import File from '@/components/File.vue'
import ScrollTop from '@/components/ScrollTop.vue'
import { getReq } from '@/api/login'

// ref 赋予原始数据类型响应式特性
// reactive 赋予对象响应式特性
const list: any = ref([]) // 数据列表
const loading = ref(false) // 加载中
const finished = ref(false) // 加载完成
const refreshing = ref(false) // 刷新状态
const searchVal = ref('') // 搜索关键词
const active = ref(0) // 当前tab
let currentTabComponent: any = reactive(Video) // 当前组件

// 不需要响应的常量，可直接渲染
const options = [
    { label: '视频', value: 'video' },
    { label: '图片', value: 'image' },
    { label: '文件', value: 'file' }
]

currentTabComponent = computed(() => {
    list.value = []
    finished.value = false
    if (active.value == 0) {
        return Video
    } else if (active.value == 1) {
        return Image
    } else {
        return File
    }
})

const onLoad = () => {
    setTimeout(() => {
        if (refreshing.value) {
            list.value = []
            refreshing.value = false
        }

        for (let i = 0; i < 10; i++) {
            list.value.push(list.value.length + 1)
        }
        loading.value = false

        if (list.value.length >= 40) {
            finished.value = true
        }
        console.log(list.value)
    }, 1000)
}

const onRefresh = () => {
    // 清空列表数据
    finished.value = false

    // 重新加载数据
    // 将 loading 设置为 true，表示处于加载状态
    loading.value = true
    onLoad()
}

onMounted(() => {
    getReq({ name: 22 })
})

const onSearch = (val: string) => {
    searchVal.value = val
    alert(val)
}
</script>

<style lang="scss" scoped>
.page {
    padding-top: calc(36px + 0.6rem);
    :deep(.van-tab--active) {
        font-weight: bold;
    }
    :deep(.van-tabs__line) {
        background-color: #4e6ef2;
    }
    :deep(.van-tabs__wrap) {
        border-bottom: 1px solid #eee;
    }
}
</style>
