<template>
    <div :class="['pillar', {'pillar-active':isActive }]" :style="{height: height + 'px'}">
        {{number}}
    </div>
</template>

<script>
    export default {
        name: 'pillar',
        props: {
            height: {
                type: Number,
                required: true,
            },
            isActive: {
                type: Boolean,
                required: true,
            },
            number: {
                type: Number,
                required: true,
            }
        },
        created() {
            
        },
        methods: {

        },
    }
</script>

<style scope lang="scss">
    .pillar{
        width :50px;
        margin-left : 50px;
        font-size : 18px;
        text-align : center;
        background-color : aqua;
    }

    // 比较中的柱子 给特殊颜色
    .pillar-active{
        background-color : greenyellow;
    }
</style>