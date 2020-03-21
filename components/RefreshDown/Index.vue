<template>
    <div
        class="Refresh-content"
        ref="scrollBox"
        :style="style"
        @touchstart="touchStart"
        @touchmove="touchMove"
        @touchend="touchEnd"
    >
        <div class="weui-pull-refreshing-box">
            <div v-if="moveState < 2">{{ moveState === 0 ? '下拉即可刷新...' : '释放即可刷新...' }}</div>
            <div v-else>
                <i class="weui-loading" /> 加载中...
            </div>
        </div>
        <div class="weui-pull-present-box">
            <slot/>
        </div>
        <div class="weui-pull-refreshing-box">
            <div v-if="moveStateTop === 4">加载中</div>
            <div v-if="moveStateTop === 5">加载完成</div>
            <div v-if="moveStateTop === 7">没有更多</div>
        </div>
    </div>
</template>

<script>
export default {
    name: "refresh",
    data() {
        return {
            duration: 0, //动画持续时间，0就是没有动画
            moveDistance: 0, //保存向下滑动的距离
            startY: "", //保存touch时的Y坐标
            moveState: 0, //开始滑动到结束后状态的变化 0:下拉即可刷新 1:释放即可刷新 2:加载中
            moveStateTop: 7
        };
    },
    computed: {
        style() {
            return {
                transition: `${this.duration}`,
                transform: `translate3d(0, ${this.moveDistance}px, 0)`
            };
        }
    },
    methods: {
        touchStart(e) {
            this.duration = 0
            this.moveDistance = 0
            this.startY = e.targetTouches[0].clientY
        },
        touchMove(e) {
            const scrollTop = document.documentElement.scrollTop || document.body.scrollTop
            if (scrollTop > 0) return
            let move = e.targetTouches[0].clientY - this.startY
            if(move > 0 && e.cancelable) {
                e.preventDefault()
                this.moveDistance = Math.pow(move, 0.8)
                if(this.moveDistance > 50) {
                    if(this.moveState === 1) return
                    this.moveState = 1
                } else {
                    if(this.moveState === 0) return
                    this.moveState = 0
                }
            }
        },
        touchEnd() {
            this.duration = 300
            if(this.moveDistance > 50) {
                this.moveState = 2
                this.moveDistance = 50
                this.$emit('refresh', () => {
                    this.moveState = 0
                    this.moveDistance = 0
                })
            } else {
                this.moveDistance = 0
            }
        }
    }
};
</script>

<style scoped lang="scss">
    $height: 50px;
    .Refresh-content {
        display: flex;
        height: calc(100vh - $height);
        flex-direction: column;
        margin-top: -$height;
        .weui-pull-refreshing-box {
            line-height: $height;
            height: $height;
            font-size: 14px;
            color: rgba(69, 90, 100, 0.6);
            text-align: center;
        }
        .weui-pull-present-box {
            background-color: lighten(#fff, 10%);
        }
    }
</style>