<template>
    <div class="bubble-sort-container">
        <div class="pillars-container">
            <div v-for="(item, index) in pillars" :key="'pillar'+index">
                <Pillar :height="item.height" :isActive="item.isActive" :number="item.number"></Pillar>
            </div>
        </div>

        <div class="bubble-sort-button">
            <button @click="initStart">开始</button>
        </div>
    </div>
</template>

<script>
    import Pillar from '../components/pillar'
    import { sleep } from '../../common/utils'
    import _ from 'lodash';
    export default {
        name: 'Home',
        data() {
            return {
                array: [4, 5, 3, 1, 9, 2, 6], // 初始化数组
                maxHeight: 500, // 设置柱子的最高高度
                perHeight: 0, // 每份柱子的高度
                pillars: [] // 柱子
            }
        },
        components: {
            Pillar
        },
        created() {

        },
        mounted() {
            /** 页面实例挂载后 先计算每个柱子的高度  然后 首次渲染pillars */
            this.perHeight = this.getPerHeight();
            this.renderPillars()
        },
        computed: {

        },
        methods: {
            /** 获取每份柱子的高度 */
            getPerHeight() {
                // 获取最大值
                let maxNumber = Math.max(...this.array);

                // 一份高度大小
                let perHeight = Math.floor(this.maxHeight / maxNumber);
                return perHeight;
            },

            /** 生成本次渲染圆柱体 数组 */
            renderPillars(actives = []) {

                let arr = [];
                _.map(this.array, (item, index) => {
                    arr.push({
                        height: parseInt(item * this.perHeight),
                        isActive: _.includes(actives, index), // _.includes(arr,a) 判断元素a 是否在arr数组中
                        number: item
                    })
                })

                this.pillars = arr;
            },

            /** 点击开始，重新赋值，重新执行冒泡 */
            initStart() {
                this.array = [4, 5, 3, 1, 9, 2, 6];
                this.startSort();
            },

            /** 冒泡排序 */
            async startSort() {
                const { array } = this;

                if (Array.isArray(array)) {

                    /* 如果不是数组，返回 */
                    if (array.length < 1) {
                        return array;
                    }

                    for (let i = 0; i < array.length - 1; i++) {

                        for (let j = 0; j < array.length - 1 - i; j++) {

                            /* 等待0.8s，渲染执行动画 */
                            await sleep(200);

                            /** 给’当前值‘和’比较值‘ active样式 */
                            this.renderPillars([j, j + 1])

                            if (array[j] <= array[j + 1]) {

                                let temp = array[j];

                                array[j] = array[j + 1];

                                array[j + 1] = temp;

                            }
                        }
                    }

                    /** 排序完成 去掉active 恢复样式 */
                    this.renderPillars()
                }
            }
        },
    }
</script>

<style scope lang="scss">
    .bubble-sort-container {
        height: 100%;
        display: flex;
        padding-top: 50px;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .pillars-container {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: flex-end;
    }

    .bubble-sort-button {
        margin-top: 60px;

        button {
            width: 64px;
            height: 36px;
            border: none;
            border-radius: 6px;
            background-color: #3f51b5;
            color: #fff;
            font-size: 16px;
            font-weight: bold;
        }
    }
</style>