<template>
    <div class="avator"></div>
</template>

<script>
export default {
    name: 'avator'
}
</script>

<style scoped lang="less">
.avator {
    background-image: url('/public/images/avator.png');
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: 0 0;
    outline: 2px solid #fff;
    transition: all .3s ease;
    transform-origin: center center;
    transform: translate(-50%, -50%);

    &:hover {
        transform: translate(-50%, -50%) scale(1.2);
        background-image: url('/public/images/avator-smile.jpg');
    }
}
</style>