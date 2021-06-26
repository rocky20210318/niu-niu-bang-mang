<template>
    <div id="help-list">
        <sticky>
            <van-row type="flex" justify="space-between" align="center" class="nav" >
                <!-- <div @click="$router.go(-1)"><van-icon name="arrow-left" class="icon" color="#fff" size="20px" /></div> -->
                <Search v-model="keys" show-action placeholder="请输入搜索关键词" class="search" @search="search" />
            </van-row>
            <dropdown-menu class="dropdown">
                <dropdown-item v-model="value1" :options="option1"  @closed="sort" />
                <dropdown-item v-model="value2" :options="option2" @closed="sort" ref="item" />
            </dropdown-menu>
        </sticky>
        <GoodsList ref="goodsList" :keys="keys" :query-fun="queryFun" :tag="value1" class="goods-list"/>
        <basic-footer />
    </div>
</template>

<script>
import { Search, DropdownMenu, DropdownItem, Sticky } from 'vant'
import GoodsList from '../components/goods-list'

export default {
    name: 'help-list',
    components: {
        Search,
        DropdownMenu,
        DropdownItem,
        Sticky,
        GoodsList
    },
    data () {
        // console.log(this.$route.query.tag)
        return {
            keys: this.$route.query.keys,
            value1: this.$route.query.tag || '',
            value2: 0,
            switch1: false,
            switch2: false,
            option1: [
                { text: '全部分类', value: '' },
                { text: '取快递', value: '快递' },
                { text: '拿外卖', value: '外卖' },
                { text: '跑腿', value: '跑腿' },
                { text: '带货', value: '带货' }
            ],
            option2: [
                { text: '最新发布', value: 0 },
                { text: '打赏最多', value: 1 }
            ],
            params: {}
        }
    },
    computed: {
        queryFun () {
            const fun = () => {
                return (Fun) => {
                    Fun.equalTo('state', 0)
                    Fun.greaterThan('endTime', new Date())
                    switch (this.value2) {
                    case 0:
                        Fun.descending('createdAt')
                        break
                    case 1:
                        Fun.descending('money')
                        break
                    }
                }
            }
            return fun()
        }
    },
    async created () {
    },
    mounted () {
    },
    methods: {
        sort () {
            this.search()
        },
        search () {
            this.$refs.goodsList.onRefresh()
            // Location.
        }
    }
}
</script>
<style lang="scss">
#help-list {
    .van-search__action {
        // color: #fff;
    }
    .van-dropdown-menu__bar {
        // background: #fff;
    }
    .van-dropdown-menu__title {
        // color: #fff;
    }
}
</style>
<style lang="scss" scoped>
#help-list {
    .nav {
        background: #fff;
        .icon {
            margin-left: 20px;
            color: #000!important;
        }
        .search {
            flex: 1;
            background: rgba(0,0,0,0);
        }
    }
    .dropdown {
        // background: linear-gradient(90deg,#fcd755 0%,#d81e06 100%)
    }
    .goods-list {
        // margin: 24px;
    }
}
</style>
