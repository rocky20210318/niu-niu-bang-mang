<template>
    <div id="order-list">
        <van-nav-bar fixed left-arrow @click-left="$router.go(-1)" placeholder title="发布的" />
        <Search v-model="keys" show-action placeholder="请输入商品标题" class="search" @search="search" />
        <tabs v-model="showType" sticky animated swipeable color="#121314" @click="onClickTab">
            <tab title="全部"/>
            <tab title="待领取"/>
            <tab title="未完成"/>
            <tab title="已完成"/>
            <tab title="已结束"/>
        </tabs>
        <GoodsList ref="goodsList" :keys="keys" :query-fun="queryFun" class="goods-list"/>
        <!-- <div><Empty description="暂无未发布"/></div> -->
    </div>
</template>

<script>
import { tabs, tab, Search } from 'vant'
import GoodsList from '../components/goods-list'
import AV from 'leancloud-storage'

export default {
    name: 'order-list',
    components: {
        tabs,
        tab,
        // Empty,
        Search,
        GoodsList
    },
    data () {
        return {
            isShowPayMethod: false,
            orderList: [],
            isShowAlter: false,
            alterData: null,
            showType: Number(this.$route.query.showType) || 0,
            keys: ''
        }
    },
    computed: {
        queryFun () {
            const fun = () => {
                return (Fun) => {
                    Fun.equalTo('createUser', AV.User.current())
                    switch (this.showType) {
                    case 1:
                        Fun.equalTo('state', 0)
                        break
                    case 2:
                        Fun.equalTo('state', 1)
                        break
                    case 3:
                        Fun.equalTo('state', 4)
                        break
                    case 4:
                        Fun.lessThan('endTime', new Date())
                        break
                    }
                }
            }
            return fun()
        }
    },
    async created () {
        // this.getOrderList()
    },
    mounted () {
    },
    methods: {
        async onClickTab (index) {
            console.log(index)
            // this.showType = index
            this.$refs.goodsList.onRefresh()
            // await this.getOrderList()
        },
        search () {
            this.$refs.goodsList.onRefresh()
        }
    }
}
</script>
<style lang="scss" scoped>
#order-list {
    .pay {
        position: absolute;
        left: 35px;
        bottom: 15px;
        width: 150px;
        height: 45px;
        border-radius: 10px;
        background: linear-gradient(to right,#ffd01e,#ff8917);
        border: 0 solid #999;
        line-height: 45px;
        color: #fff;
    }
    .button {
        width: 180px;
        height: 50px;
        background-color: #f1f1f1;
        box-shadow: 0px 2px 10px 0px rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        border: 0;
        color: #6e6e6e;
        line-height: 50px;
        // position: absolute;
        // right: 0px;
        // bottom: 10px;
        // width: 200px;
        // height: 45px;
        // border-radius: 10px;
        // background: #fff;
        // border: 0 solid #999;
        // line-height: 45px;
        // color: #999;
    }
    .order-item {
        position: relative;
        margin: 20px 20px 10px;
        background: #fff;
        border-radius: 15px;
        .text {
            padding: 20px 40px 0;
            font-size: 28px;
            color: #888;
        }
        .list {
            padding: 0;
        }
        .item {
            // border: 1px solid #eee;
            // background: #f7f7f7;
        }
    }
    .payMethod {
        position: fixed;
        z-index: 99999;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.7);
        .payList {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            background: #fff;
            font-size: 34px;
            text-align: center;
            li {
                height: 150px;
                border-bottom:  1px solid #eee;
                line-height: 150px;
            }
            img {
                max-width: 80px;
                max-height: 80px;
                vertical-align: middle;
                margin-right: 20px;
                margin-top: -10px;
            }
            a {
                display: block;
            }
        }
    }
    .book-detail {
        padding: 20px;
        padding-top: 0;
    }
}
</style>
