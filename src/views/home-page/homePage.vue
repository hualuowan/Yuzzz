<template>
    <main>
        <Avatar class="avatar"></Avatar>
        <div class="p-container">
            <p>你好 Welcome</p>
            <p>我是一个急速成长的前端小白</p>
            <p>A rapidly growing front-end engineer</p>
            <p class="name">--Yuzzz</p>
        </div>
    </main>
</template>

<script>
import Avatar from '@/components/avatar.vue'
export default {
    name: 'home-page',
    components: {
        Avatar
    }
}
</script>

<style scoped lang="less">
main {
    width: 100%;
    height: 100%;
    background-blend-mode: multiply;
    position: relative;
    overflow: hidden;
    background-color: transparent;

    .avatar {
        position: absolute;
        left: 50%;
        top: 25%;
        transform: translate(-50%, -50%);
        animation: movedown .6s ease-in-out;
    }

    .p-container {
        padding-top: 40vh;

        p {
            font-size: 34px;
            font-weight: 800;
            color: #fff;
            width: 100%;
            text-align: center;
            margin-top: 2%;
            transform: translate(0, 600px);
            user-select: none;
        }

        each(range(4), {

            p:nth-child(@{value}) {
                animation: moveup .6s ((@value - 1)* .1s) ease-in-out forwards;
            }
        });

    .name {
        padding-left: 40%;
    }
}
}

@keyframes movedown {
    0% {
        opacity: 0;
        transform: translate(-50%, -200px);
    }

    100% {
        opacity: 1;
        transform: translate(-50%, -50%);
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