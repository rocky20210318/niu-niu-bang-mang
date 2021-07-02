<template>
    <router-link :to="`/details/${details.id}`" class="item">
        <van-row type="flex" justify="space-between" align="center" class="title-name">
            <p class="title van-ellipsis">{{ details.title }}</p>
            <!-- <img :src="details.userImage" class="img"> -->
            <van-image :src="details.userImage" round fit="fill" class="img" />
            <p class="name van-ellipsis">{{ details.username }}</p>
        </van-row>
        <van-row type="flex" justify="space-between" align="center" class="address-tag">
            <p class="tag">{{ details.tag }}</p>
            <p class="endTime">{{ format(details.endTime, 'YYYY-MM-dd HH:mm') }}</p>
        </van-row>
        <van-row type="flex" justify="space-between" align="center" class="">
            <div class="img"><img :src="imgSrc" alt=""></div>
            <p class="money">¥<span>{{ details.money }}</span></p>
        </van-row>
    </router-link>
</template>

<script>
// import { Tag } from 'vant'
import { addCart } from '../../services'
import { format } from '../../utils/index'
import AV from 'leancloud-storage'

export default {
    name: 'goods-list-item-1',
    props: {
        details: {
            type: Object,
            default () {
                return {}
            }
        }
    },
    components: {
        // Tag
    },
    data () {
        return {
        }
    },
    computed: {
        imgSrc () {
            let url = require('../../assets/bangmang.png')
            switch (this.details.tag) {
            case '快递':
                url = require('../../assets/kuaidi.png')
                break
            case '外卖':
                url = require('../../assets/waimai.png')
                break
            case '跑腿':
                url = require('../../assets/paotui.png')
                break
            case '带货':
                url = require('../../assets/daihuo.png')
                break
            }
            return url
        }
    },
    async created () {
    },
    mounted () {
    },
    methods: {
        onAddCart (item) {
            if (AV.User.current) {
                addCart(item)
                this.$toast('加入购物车成功')
            } else this.$router.push('/login')
        },
        format (date, fm) {
            return date ? format(date, fm) : '长期有效'
        }
    }
}
</script>
<style lang="scss" scoped>
.item {
    display: block;
    margin: 30px;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0px 0rem 0.13333rem 1px rgb(0 0 0 / 6%);
    background: #fff;
    font-size: 28px;
    color: #888;
    line-height: 1;
    .title-name {
        margin-bottom: 20px;
        .img {
            position: relative;
            left: 14px;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            overflow: hidden;
            // margin-right: 10px;
        }
        .name {
            max-width: 130px;
        }
    }
    .title {
        width: 400px;
        line-height: 1.2;
        // margin-bottom: 20px;
        font-size: 36px;
        font-weight: 400;
        color: #333;
    }
    .address-tag {
        margin-bottom: 20px;
    }
    .money {
        font-size: 30px;
        color: #ff202c;
        span {
            margin-left: 4px;
            font-size: 44px;
        }
    }
    .img {
        width: 60px;
        height: 60px;
        img {
            max-width: 100%;
            max-height: 100%;
            // vertical-align: middle;
        }
    }
}
</style>
