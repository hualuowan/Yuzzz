<template>
    <main>
        <div class="canvas-container" ref="canvas">
            <Particle :now="now" :nowIndex="props.nowIndex" class="canvas"></Particle>
        </div>
        <div class="select-container">
            <div v-for="(item, index) in list" :key="item.label" class="item" :class="{
                active: now === index
            }" @click="now = index">
                <span>{{ item.label }}</span>
                <img :src="item.src" alt="">
                <a v-if="item.type" :href="item.url" target="_blank">{{ item.url }}</a>
                <p v-else>{{ item.url }}</p>
            </div>
        </div>
    </main>
</template>

<script setup>
import Particle from '@/components/particle.vue';
import { ref, defineProps } from 'vue';

const now = ref(0)

const list = [
    { label: "wechat", src: "/images/wechat.png", url: "a517292656", type: 0 },
    { label: "gitee", src: "/images/gitee.jpg", url: "https://gitee.com/a517292656/knowledge-base", type: 1 },
    { label: "email", src: "/images/email.png", url: "517292656@qq.com", type: 0 },
    { label: "csdn", src: "/images/csdn.png", url: "https://blog.csdn.net/weixin_60599861", type: 1 },
    { label: "leetcode", src: "/images/leetcode.png", url: "https://leetcode.cn/u/hllx/", type: 1 },
]

const props = defineProps({
    nowIndex: {
        type: Number,
        default: 0
    }
})

</script>

<script>
export default {
    name: 'contact',
}
</script>

<style scoped lang="less">
main {
    width: 100%;
    height: 100%;
    display: flex;

    .canvas-container {
        width: 50%;
        height: 100%;

        .canvas {
            width: 50%;
            height: 50%;
            background-color: #000;
        }
    }

    .select-container {
        width: 50%;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        align-items: start;

        .item {
            width: 70%;
            height: 60px;
            color: #fff;
            display: flex;
            justify-content: start;
            align-items: center;

            span {
                margin-left: 20px;
                width: 60px;
            }

            img {
                width: 40px;
                height: 40px;
                margin: 0 10px;
            }

            a {
                color: #fff;
            }

            &.active {
                border-radius: 5px;
                position: relative;
                overflow: hidden;
                z-index: 1;

                &::before {
                    content: "";
                    width: 200%;
                    height: 500%;
                    position: absolute;
                    background-color: rgba(71, 185, 232);
                    z-index: -2;
                    left: 50%;
                    top: 50%;
                    transform-origin: top left;
                    animation: rotate 3s linear infinite;
                }

                &::after {
                    content: "";
                    width: calc(100% - 4px);
                    height: calc(100% - 4px);
                    position: absolute;
                    left: 2px;
                    top: 2px;
                    background-color: #000;
                    border-radius: 5px;
                    z-index: -1;
                }
            }
        }
    }
}

@keyframes rotate {
    from {
        transform: rotate(0deg);
    }

    to {
        transform: rotate(360deg);
    }
}
</style>