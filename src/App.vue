<template>
  <div id="app" ref="app">
    <div class="container" ref="container">
      <component :is="item.component" v-for="(item) in views" :key="item.name" :style="item.style"
        @click="scroll(item.top)" :nowIndex="nowIndex"></component>
    </div>
    <div class="side-bar">
      <Sider :list="list" @page="changePage" :now="nowIndex"></Sider>
    </div>
    <Stars class="star"></Stars>
  </div>
</template>

<script setup>
import { ref, onMounted, getCurrentInstance } from "vue";
import selfIntroduction from "@/views/self-introduction/selfIntroduction.vue"
import homePage from "@/views/home-page/homePage.vue"
import jobExperience from "@/views/job-experience/jobExperience.vue"
import contact from "@/views/contact/contact.vue"
import Sider from "@/components/sider.vue"
import Stars from "@/components/stars.vue"

const nowIndex = ref(0)

const views = [
  {
    name: 'homePage',
    component: homePage,
    style: {
      width: '100vw',
      height: '100vh',
    },
    top: 0,
    tag: '首页'
  },
  {
    name: 'selfIntroduction',
    component: selfIntroduction,
    style: {
      height: '100vh',
      width: '100vw',
    },
    top: 100,
    tag: '自我介绍'
  },
  {
    name: 'jobExperience',
    component: jobExperience,
    style: {
      width: '92vw',
      height: '100vh',
    },
    top: 200,
    tag: '实习&项目经历'
  },
  {
    name: 'contact',
    component: contact,
    style: {
      height: '100vh',
      width: '100vw'
    },
    top: 300,
    tag: '联系方式'
  }
]

const list = views.map(e => e.tag)

const h = ref(0)
const container = ref(null)
const app = ref(null)

function changePage(index) {
  nowIndex.value = index
  throggleScroll(null, index)
}

onMounted(() => {
  h.value = window.innerHeight
  let current = getCurrentInstance()
  container.value = current.refs.container
  app.value = current.refs.app

  app.value.onmousewheel = function (e) {
    let direction = e.deltaY >= 0 ? 'down' : 'up'
    throggleScroll(direction)
    return false
  }
})

function scroll(direction, index) {
  if (direction === 'down' && nowIndex.value < views.length - 1) {
    nowIndex.value++
  } else if (direction === 'up' && nowIndex.value > 0) {
    nowIndex.value--
  }
  if (index !== undefined) nowIndex.value = index
  console.log(nowIndex.value)
  app.value.scrollTo({
    top: h.value * views[nowIndex.value].top / 100,
    behavior: 'smooth'
  })
}

// 节流
function throggle(func, delay) {
  let timer = null;
  return function (...args) {
    if (timer) return
    timer = setTimeout(() => {
      func(...args)
      clearTimeout(timer)
      timer = null
    }, delay);
  };
}

const throggleScroll = throggle(scroll, 500)
</script>

<script >
export default {
  name: 'app'
}
</script>

<style lang="less">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

::-webkit-scrollbar {
  display: none;
}

#app {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: row;
  overflow-y: scroll;
  overflow-x: hidden;

  .container {
    width: 90%;
    height: 500vh;
  }
}

.side-bar {
  position: fixed;
  right: 0;
  top: 0;
  width: 8%;
  height: 100vh;
}

.star {
  width: 100vw;
  height: 100vh;
  position: fixed;
  z-index: -4;
  left: 0;
  top: 0;
}
</style>