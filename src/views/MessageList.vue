<template>
    <div id="message-list">
        <van-nav-bar class="nav" :border="false" fixed left-arrow placeholder @click-left="$router.go(-1)" title="消息中心"/>
        <Tabs color="#313635" animated>
            <Tab title="通知">
                <empty
                    class="custom-image"
                    image="https://img.yzcdn.cn/vant/custom-empty-image.png"
                    description="暂无消息"
                />
            </Tab>
            <Tab title="公告">
                <empty
                    class="custom-image"
                    image="https://img.yzcdn.cn/vant/custom-empty-image.png"
                    description="暂无消息"
                />
            </Tab>
        </Tabs>
    </div>
</template>

<script>
import { Tab, Tabs, Empty } from 'vant'

export default {
    name: 'message-list',
    components: {
        Tab,
        Tabs,
        Empty
    },
    data () {
        return {
            active: 0
        }
    },
    computed: {
    },
    async created () {
    },
    mounted () {
    },
    methods: {
    }
}
</script>
<style lang="scss">
#message-list {
    .custom-image .van-empty__image {
        width: 200px;
        height: 200px;
        margin-top: 200px;
    }
}
</style>
<style lang="scss" scoped>
.banner-box {
    margin: 20px;
    img {
        width: 100%;
        margin-bottom: 10px;
        border-radius: 2    0px;
    }
}
</style>
