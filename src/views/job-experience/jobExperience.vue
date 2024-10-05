<template>
    <main>
        <div class="shell">
            <div class="box" v-for="item in exp" :key="item.name">
                <div class="content">
                    <div class="title">
                        <p>{{ item.name }}</p>
                    </div>
                    <div class="exp-container">
                        <div class="items" v-for="p in item.content" :key="p">{{ p }}</div>
                    </div>
                </div>
                <span>{{ item.type }}</span>
            </div>
        </div>
    </main>
</template>


<script setup>
const exp = [
    {
        name: "金山办公",
        type: '实习',
        content: [
            "2024-07 ~ 2024-09",
            "教育产研部 前端开发实习生",
            "封装了右键菜单组件和文件夹选取组件",
            "参考 webpack5 的持久化缓存技术对判分逻辑进行优化",
            "利用异步渲染进行组件异步展示",
            "为界面添加样式，如下拉动画、自适应背景色等"
        ]
    },
    {
        name: "广东省二十支商业连锁有限公司",
        type: '实习',
        content: [
            "2024-03 ~ 2024-06",
            "信息部 前端开发实习生",
            "参与公司内部的广告模块的开发，包括前台的广告播放器 和 后台的广告管理系统",
            "参与了公司 BFF 层的设计与开发，利用 BFF 层完成了文件上传协议的制定",
            "提出利用散列组合和位运算精简权限组合",
            "使用 ECharts 生成报表",
        ]
    },
    {
        name: "金山办公教育插件",
        type: '项目',
        content: [
            "2024-07 ~ 2024-09",
            "项目介绍: 金山教育组的内部使用出题插件(B 端)，现在想开放给 C 端使用，因此需要优化",
            "编写了自动化测试脚本(node 环境),发现并记录问题，参考 webpack5 的持久化缓存技术对判分逻辑进行优化，使判分时间下降 20%",
            "封装了右键菜单组件和文件夹选取(拖拽上传)组件",
            "采用异步渲染的方式对组件进行异步展示",
            "为界面添加样式，主要是下拉动画和自适应背景色"
        ]
    },
    {
        name: "二十支广告终端",
        type: '项目',
        content: [
            "2024-03 ~ 2024-04",
            "项目介绍: 公司内部的广告播放器，C 端产品，使用 VUE 开发",
            "广告文件本地化存储，利用了 blob 的分片机制存入 indexedb 中，减少了服务器流量消耗，使非首次启动的加载时间提升 90%",
            "对广告播放做了界面展示的判断(最小化自动暂停，显示自动播放)",
        ]
    },
    {
        name: "二十支广告后台",
        type: '项目',
        content: [
            "2024-04 ~ 2024-06",
            "项目描述: 公司内部的广告后台管理系统，包含广告控制、报表展示等动能",
            "对 RBAC 的权限组合逻辑进行了优化，提出利用散列组合和位运算精简权限组合",
            "参与了多种类型文件预览的功能设计，包括图片、视频、office、pdf、markdown 等文件",
            "参与了大文件分片上传功能的设计与实现，加快了大文件上传速度并且减少了服务器资源的占用",
            "封装了动态表格(后面的选项会根据前面的选择改变)组件",
        ]
    },
]
exp
</script>

<script>
export default {
    name: 'jobExperience'
}
</script>

<style scoped lang="less">
main {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 90%;
    height: 100vh;

    .shell {
        width: 90%;
        height: 700px;
        display: flex;
    }

    .box {
        flex: 1;
        overflow: hidden;
        transition: .5s;
        margin: 0 20px;
        box-shadow: 10px 10px 20px rgba(0, 0, 0, .5);
        border-radius: 20px;
        border: 10px solid #fff;
        background-color: #fff;

        .content {
            height: 85%;
            object-fit: cover;
            transition: all .5s ease;
            background-image: url('/public/images/exp-bg.jpg');
            background-position: center center;;
            display: flex;

            .title {
                width: 100%;
                height: 85%;
                font-size: 30px;
                color: #000;
                display: flex;
                justify-content: center;
                transition: .5s;

                p {
                    writing-mode: vertical-rl;
                    text-orientation: upright;
                    text-align: center;
                }
            }

            .exp-container {
                width: 0;
                overflow: hidden;
            }
        }

        span {
            font: 200 45px '优设标题黑';
            text-align: center;
            height: 15%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        &:hover {
            flex-basis: 40%;

            &>.content {
                width: 100%;
                height: 100%;

                .title {
                    width: 20%;
                }

                .exp-container {
                    width: 80%;
                    height: 100%;
                    padding: 10%;

                    .items {
                        font-size: 16px;
                        margin-top: 10px;
                        overflow: hidden;
                    }
                }
            }
        }
    }





}
</style>