<template>
    <div class="water-fall">
        <div class="water-fall-cell" v-for="item in list">
            <img src="https://img.yzcdn.cn/vant/cat.jpeg" @click="preview('https://img.yzcdn.cn/vant/cat.jpeg')" alt="加载失败" />
            <p class="cell-title">图片名称图片名称图片名称图片名称{{ item }}</p>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Image as VanImage, ImagePreview } from 'vant'

// 使用 defineProps 声明 props
const props = defineProps({
    list: {
        type: Array,
        default: () => []
    }
})

const preview = src => {
    ImagePreview({
        images: [src],
        closeable: true,
        showIndex: false
    })
}
</script>

<style lang="scss" scoped>
.water-fall {
    width: calc(100% - 0.15rem);
    margin: 0.15rem auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}
.water-fall-cell {
    width: calc(50% - 0.15rem);
    padding: 0.2rem;
    margin: 0 0.075rem 0.15rem;
    border-radius: 0.12rem;
    background-color: #fff;
    box-sizing: border-box;
}
.water-fall-cell img {
    width: 100%;
    border-radius: 0.1rem;
}
.water-fall-cell p {
    font-size: 0.32rem;
    margin-top: 0.1rem;
    line-height: 1.5;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    overflow: hidden;
    text-align: left;
}
</style>
