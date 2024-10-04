<template>
    <div id="canvas-3e4c5d"></div>
</template>

<script setup>
import { onMounted } from 'vue';


onMounted(() => {
    let dom = document.querySelector('#canvas-3e4c5d')

    let canvas = document.createElement('canvas')
    let ctx = canvas.getContext('2d')


    let w = canvas.width = window.innerWidth
    let h = canvas.height = window.innerHeight

    let hue = 217
    let stars = []
    let count = 0
    let maxStars = 1200

    // 2号画布
    let canvas2 = document.createElement('canvas')
    let ctx2 = canvas2.getContext('2d')

    canvas2.width = 100
    canvas2.height = 100

    let half = canvas2.width / 2
    // 创建矩形颜色渐变
    let gradient2 = ctx2.createRadialGradient(half, half, 0, half, half, half)

    // 添加色阶 仓蓝色
    gradient2.addColorStop(0.025, '#fff')
    gradient2.addColorStop(0.1, `hsl(${hue},61%,33%)`)
    gradient2.addColorStop(0.25, `hsl(${hue},64%,6%)`)
    gradient2.addColorStop(1, 'transparent')


    ctx2.fillStyle = gradient2
    ctx2.beginPath()
    // 画圆形
    ctx2.arc(half, half, half, 0, Math.PI * 2)
    ctx2.fill()

    // 星星类
    class Star {
        constructor() {
            this.orbitRadius = random(maxOrbit(w, h)) // 轨道直径
            this.radius = random(60, this.orbitRadius) / 12 // 自身半径
            this.orbitX = w / 2
            this.orbitY = h / 2
            // 这里的速度计算 => timePassed = timePassed + speed 
            // 因为运动轨迹是圈圈 所以拿到当前的timePassed 相当于 拿到 当前角度 可以计算出 当前位置
            this.timePassed = random(0, maxStars)// 自身度过时间
            this.speed = random(this.orbitRadius) / 500000 // 速度
            this.alpha = random(2, 10) / 10 // 透明度 => 闪烁

            count++
            stars[count] = this
        }
        // 绘制函数
        draw() {
            // 计算出当前的x y
            let x = Math.sin(this.timePassed) * this.orbitRadius + this.orbitX
            let y = Math.cos(this.timePassed) * this.orbitRadius + this.orbitY
            let twinkle = random(10) // 闪烁

            if (twinkle == 1 && this.alpha > 0) {
                this.alpha -= 0.05
            }
            else if (twinkle == 2 && this.alpha < 1) {
                this.alpha += 0.05
            }

            // 设置透明度
            ctx.globalAlpha = this.alpha
            // 把ctx2的图像绘制到ctx上
            // img x y width height 
            // 在这里定义宽高 所以之前的图像尺寸没有影响
            ctx.drawImage(canvas2, x - this.radius / 2, y - this.radius / 2, this.radius, this.radius)
            this.timePassed += this.speed
        }
    }

    for (let i = 0; i < maxStars; i++) {
        new Star()
    }

    // 动画函数
    function animation() {
        // 默认 画背景
        ctx.globalCompositeOperation = 'source-over'
        ctx.globalAlpha = 0.8
        ctx.fillStyle = "hsla(" + hue + ", 64%, 6%, 1)"
        ctx.fillRect(0, 0, w, h)

        // 原图+目标图
        ctx.globalCompositeOperation = 'lighter'
        stars.forEach(star => {
            star.draw()
        })

        window.requestAnimationFrame(animation)
    }

    animation()

    dom.appendChild(canvas)
})

// 随机函数
function random(min, max) {
    if (arguments.length < 2) {
        max = min
        min = 0
    }
    if (min > max) {
        var t = min
        min = max
        max = t
    }

    return Math.floor(Math.random() * (max - min + 1)) + min
}

// 最大轨道（轨道直径）
function maxOrbit(x, y) {
    let max = Math.max(x, y)
    let diameter = Math.round(Math.sqrt(max ** 2 + max ** 2)) // 直径
    return diameter / 2
}
</script>

<script>
export default {
    name: 'stars'
}
</script>

<style scoped>
#canvas {
    width: 100vw;
    height: 100vh;
}
</style>