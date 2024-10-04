<template>
    <main>
        <h2>
            <div class="title">
                ABOUT ME
            </div>
        </h2>
        <div class="main-container">
            <div class="left-box" :class="{ active: props.nowIndex === 1 || flag }">
                <avatar class="avatar"></avatar>
                <div class="info-box">
                    <div class="info-item" v-for="item in info" :key="item.title">
                        {{ item.title }} : {{ item.content }}
                    </div>
                </div>
            </div>
            <div class="right-box" :class="{ active: props.nowIndex === 1 || flag }">
                <div class="radio-box" v-for="item in skills" :key="item.title">
                    <p>{{ item.title }}</p>
                    <div class="progress">
                        <div class="progress-bar" :style="{ width: item.progress + '%' }"></div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
import Avatar from "@/components/avatar.vue"
import { defineProps, ref, watch } from 'vue'
const props = defineProps({
    nowIndex: {
        type: Number,
        default: 0
    }
})

const flag = ref(0)

watch(props, () => {
    if(props.nowIndex === 1) {
        flag.value = 1
    }
})

const info = [
    {
        title: 'Name',
        content: 'Yuzzz'
    },
    {
        title: 'University',
        content: '华南师范大学(211)'
    },
    {
        title: 'Major',
        content: '计算机科学与技术'
    },
    {
        title: 'GraduationYear',
        content: '2025'
    },
    {
        title: 'Looking for',
        content: '前端开发工程师'
    }
]

const skills = [
    {
        title: 'CSS',
        progress: 60
    },
    {
        title: 'JavaScript',
        progress: 95
    },
    {
        title: 'Vue.js',
        progress: 80
    },
    {
        title: 'Node.js',
        progress: 80
    },
    {
        title: 'Webpack',
        progress: 80
    }
]
</script>

<script>
export default {
    name: 'selfIntroduction'
}
</script>

<style scoped lang="less">
main {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    position: relative;
    justify-content: space-between;
    overflow: hidden;
    background-color: transparent;

    h2 {
        width: 100%;
        height: 100px;
        margin-top: 3%;
        display: flex;
        justify-content: center;
        color: #fff;

        .title {
            width: 30%;
            height: 100%;
            text-align: center;
            line-height: 100px;
            font-size: 40px;
            border-image: linear-gradient(to right, rgba(70, 70, 70, 0) 0%, #464646 50%, rgba(70, 70, 70, 0) 100%) 30 stretch;
            border-top: 1px solid;
            border-bottom: 1px solid;
        }
    }

    .main-container {
        width: 100vw;
        height: 75%;
        margin: 0;
        display: flex;
        flex-direction: row;
        padding: 0 10%;
        color: #fff;

        .left-box {
            width: 50%;
            height: 100%;
            position: relative;
            transform: translate(0, 600px);
            transition: all .6s ease;

            &.active {
                animation: moveup .6s .2s ease-in-out forwards;
                transition: all .6s ease;
            }

            .avatar {
                position: absolute;
                left: 50%;
                top: 10%;
            }

            .info-box {
                margin-top: 20%;
                padding: 0 10%;

                .info-item {
                    margin-bottom: 40px;
                    padding-left: 5px;
                    font-size: 30px;
                    border-image: linear-gradient(to right, #464646 0%, #464646 60%, rgba(70, 70, 70, 0) 100%) 30 stretch;
                    border-left-width: 0;
                    border-top-width: 0;
                    border-right-width: 0;
                    border-bottom: 1px solid;
                }
            }
        }

        .right-box {
            width: 50%;
            height: 100%;
            padding: 3%;
            display: flex;
            flex-direction: column;
            justify-content: space-evenly;
            transform: translate(0, 600px);
            transition: all .6s ease;

            &.active {
                animation: moveup .6s .4s ease-in-out forwards;
                transition: all .6s ease;

            }

            .radio-box {
                width: 100%;
                height: 60px;

                p {
                    font-size: 16px;
                    height: 20px;
                }

                .progress {
                    width: 100%;
                    height: 40px;
                    background-color: rgb(233, 236, 239);

                    .progress-bar {
                        height: 100%;
                        background-color: rgb(140, 209, 197);
                    }
                }
            }
        }
    }
}

@keyframes moveup {
    0% {
        opacity: 0;
        transform: translate(0, 600px);
    }

    100% {
        opacity: 1;
        transform: translate(0, 0);
    }
}
</style>