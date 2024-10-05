<template>
    <div id="canvas"></div>
</template>

<script setup>
import { ref, onMounted, defineProps, watch } from "vue";

const props = defineProps({
    now: Number,
    nowIndex: Number,
})

const pc = ref(null)
const step = ref(20)

watch(props, () => {
    if (props.nowIndex === 3) {
        pc.value.changeImage(logoImgs[props.now])
        pc.value.drawCanvas()
    }
})

// 图片集合
const logos = [
    { label: "wechat", url: "/images/wechat.png" },
    { label: "gitee", url: "/images/gitee.jpg" },
    { label: "email", url: "/images/email.png" },
    { label: "csdn", url: "/images/csdn.png" },
    { label: "leetcode", url: "/images/leetcode.png" },
]

/** 存储由logos生成的logoImg对象 */
const logoImgs = []

let width = window.innerWidth / 2
let height = window.innerHeight

// 获取canvas画布
var canvas = document.createElement('canvas')
canvas.height = height;
canvas.width = width;
canvas.setAttribute('style', 'position: absolute;left: 0;top: 0;pointer-events: none;');
canvas.setAttribute('id', 'canvas_sakura');


// 获取上下文
const cxt = canvas.getContext('2d');

onMounted(() => {
    let dom = document.getElementById('canvas')
    dom.appendChild(canvas);

    // 将logo数据实例化为logoImg对象
    for (let item of logos) {
        logoImgs.push(new LogoImg(item.url, item.label));
    }

    pc.value = new ParticleCanvas(canvas)
})

// 标记激活logo
// let activeLogo

// 设置粒子动画时长
const animateTime = 30;
const opacityStep = 1 / animateTime;

/** 中心影响的半径 */
// 鼠标影响半径
const Radius = 40;
/** 排斥/吸引 力度 */
const Inten = 0.95;



// 粒子类
class Particle {
    // x: number; // 粒子x轴的初始位置
    // y: number; // 粒子y轴的初始位置
    // totalX: number; // 粒子x轴的目标位置
    // totalY: number; // 粒子y轴的目标位置
    // mx?: number; // 粒子x轴需要移动的距离
    // my?: number; // 粒子y轴需要移动的距离
    // vx?: number; // 粒子x轴移动速度
    // vy?: number; // 粒子y轴移动速度
    // time: number; // 粒子移动耗时
    // r: number; // 粒子的半径
    // color: number[]; // 粒子的颜色
    // opacity: number; // 粒子的透明度
    constructor(totalX, totalY, time, color) {
        // 初始位置
        this.x = (Math.random() * width) >> 0
        this.y = (Math.random() * height) >> 0
        // 目标位置
        this.totalX = totalX
        this.totalY = totalY
        // 移动时间
        this.time = time
        // 颜色半径
        this.r = 1.2
        // 这里的颜色是在imgData中把 r g b a 分别读出来之后拼接的
        this.color = [...color]
        this.opacity = 0
        this.opacityStep = opacityStep
    }
    // 绘制函数
    draw() {
        cxt.fillStyle = `rgba(${this.color.toString()})`
        // 为什么画矩形呢？
        cxt.fillRect(this.x, this.y, this.r * 2, this.r * 2)
        // cxt.arc(this.x, this.y, this.r, 0, 2 * Math.PI);
        cxt.fill()
    }
    // 更新粒子 mouseX mouseY 鼠标位置
    update(mouseX, mouseY) {
        // 需要移动的距离
        this.mx = this.totalX - this.x
        this.my = this.totalY - this.y
        // 移动速度
        this.vx = this.mx / this.time
        this.vy = this.my / this.time
        // // 排斥效果
        // 计算粒子和鼠标的距离
        if (mouseX && mouseY) {
            let dx = mouseX - this.x
            let dy = mouseY - this.y
            let distance = Math.sqrt(dx ** 2 + dy ** 2)
            // 相对距离的比例 判断受力
            let disPercent = Radius / distance
            disPercent = disPercent > 7 ? 7 : disPercent
            // 夹角 正弦 余弦
            let angle = Math.atan2(dy, dx)
            let cos = Math.cos(angle)
            let sin = Math.sin(angle)
            // 力度转化为速度
            let repX = cos * disPercent * -Inten
            let repY = sin * disPercent * -Inten
            this.vx += repX
            this.vy += repY
            if (disPercent > 12) {
                this.vx -= repX
                this.vy -= repY
            }
        }
        this.x += this.vx
        this.y += this.vy
        // 透明度变化
        if (this.opacity < 1) {
            this.opacity += this.opacityStep
        }
    }
    // 切换粒子
    change(x, y, color) {
        this.totalX = x
        this.totalY = y
        this.color = [...color]
    }
}

// 图片类
class LogoImg {
    // src: string;
    // name: string;
    // particleData: Particle[]; // 用于保存筛选后的粒子
    constructor(src, name) {
        this.src = src
        this.name = name
        this.particleData = []
        let img = new Image()
        // 允许跨域
        img.crossOrigin = ""
        img.src = src

        // canvas解析数据源（图片）获取粒子数据
        img.onload = () => {
            requestIdleCallback(() => {
                // 获取图片像素数据
                // 创建临时canvas
                const tmp_canvas = document.createElement('canvas')
                const tmp_cxt = tmp_canvas.getContext('2d')
                const imgw = width
                const imgh = ~~(width * (img.height / img.width))
                tmp_canvas.width = imgw
                tmp_canvas.height = imgh
                tmp_cxt.drawImage(img, 0, 0, imgw, imgh)
                // 获取像素点数据
                const imgData = tmp_cxt.getImageData(0, 0, imgw, imgh).data
                tmp_cxt.clearRect(0, 0, width, height)

                // 筛选像素点
                for (let y = 0; y < imgh; y += step.value) {
                    for (let x = 0; x < imgw; x += step.value) {
                        // 像素点序号
                        const index = (x + y * imgw) * 4
                        // 在数组中对应的值 rgba 颜色值
                        const r = imgData[index]
                        const g = imgData[index + 1]
                        const b = imgData[index + 2]
                        const a = imgData[index + 3]
                        const sum = r + g + b + a
                        // 筛选条件
                        if (sum >= 100) {
                            // 新建粒子
                            const particle = new Particle(x, y, animateTime, [r, g, b, a])
                            this.particleData.push(particle)
                        }
                    }
                }
            })
        }
    }
}

// 画布类
class ParticleCanvas {
    // canvasEle: HTMLCanvasElement; // 画布对象
    // ctx: CanvasRenderingContext2D; //  cxt2d 画布上下文
    // width: number;
    // height: number;
    // particleList: Particle[]; // 粒子数组
    // mouseX?: number; // 鼠标X轴位置
    // mouseY?: number; // 鼠标Y轴位置
    constructor(target) {
        // 设置画布 获取上下文
        this.canvasEle = target
        this.cxt = target.getContext('2d')
        this.width = target.width
        this.height = target.height
        this.particleList = []
        // 监听鼠标移动
        // this.canvasEle.addEventListener('mousemove', (e) => {
        //     const { left, top } = this.canvasEle.getBoundingClientRect();
        //     const { clientX, clientY } = e;
        //     this.mouseX = clientX - left;
        //     this.mouseY = clientY - top;
        // })
        // 鼠标移出
        // this.canvasEle.addEventListener('mouseleave', (e) => {
        //     this.mouseX = 0;
        //     this.mouseY = 0;
        // })
    }
    // 改变图片
    changeImage(img) {
        // 已存在图片 => 粒子数组长度不为0
        // if (this.particleList.length) {
        //     // 获取新旧粒子数组及长度
        //     let newList = img.particleData
        //     let newLen = newList.length
        //     let oldList = this.particleList
        //     let oldLen = oldList.length

        //     // // 调用change修改已经存在的粒子 直接去新位置
        //     // for (let id = 0; id < newLen; id++) {
        //     //     // 新的
        //     //     let totalX = newList[id].totalX
        //     //     let totalY = newList[id].totalY
        //     //     let color = newList[id].color
        //     //     // 旧的数组未越界
        //     //     if (oldList[id]) {
        //     //         oldList[id].change(totalX, totalY, color)
        //     //     }
        //     //     // 旧的数组越界 生成新的
        //     //     else {
        //     //         oldList[id] = new Particle(totalX, totalY, animateTime, color)
        //     //     }
        //     // }

        //     this.particleList = []

        //     setTimeout(() => {
        //         newList.forEach(
        //             (item) => {
        //                 this.particleList.push(new Particle(item.totalX, item.totalY, animateTime, item.color))
        //             }
        //         )
        //     }, 200);

        //     // 随机打乱粒子位置
        // }
        // // 是新的 不存在旧图片
        // else {
        //     // 太大了 没塞进去
        //     setTimeout(() => {
        //         img.particleData.forEach(
        //             (item) => {
        //                 this.particleList.push(new Particle(item.totalX, item.totalY, animateTime, item.color))
        //             }
        //         )
        //     }, 200);
        // }

        // 先清空
        cxt.clearRect(0, 0, width * 2, 2 * height)
        this.particleList = []
        setTimeout(() => {
            img.particleData.forEach(
                (item) => {
                    this.particleList.push(new Particle(item.totalX, item.totalY, animateTime, item.color))
                }
            )
        }, 200);
    }
    // 画画布函数
    drawCanvas() {
        this.cxt.clearRect(0, 0, this.width, this.height)
        // setInterval(() => {
        this.particleList.forEach((particle) => {
            particle.update(this.mouseX, this.mouseY)
            particle.draw()
        })
        // }, 10);
        // 动画函数
        window.requestAnimationFrame(() => {
            this.drawCanvas()
        })
    }
}

</script>

<script>
export default {
    name: 'particle'
}
</script>

<style scoped>
#canvas {
    width: 100%;
    height: 100%;
    position: relative;
    transform: scale(.8);
}
</style>