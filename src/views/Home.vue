<template>
    <div id="home">
        <div class="banner">
            <img src="../assets/banner-1.jpg" alt="banner-1">
        </div>
        <div class="banner-2"><img src="" alt=""></div>
        <div class="list">
            <p class="title">常见互帮</p>
            <van-row type="flex" justify="space-between" align="center" class="">
                <van-row type="flex" justify="space-between" align="center" class="nav" @click="$router.push('/help-list?tag=快递')">
                    <div class="item">
                        <van-row type="flex" justify="" align="center" class="top">
                            <div class="img"><img src="../assets/kuaidi.png" alt=""></div>
                            <div class="">
                                <p class="name">帮取快递</p>
                                <p class="">已帮<span>35</span></p>
                            </div>
                        </van-row>
                        <p class="text">快递拿到手软叫兄弟来帮忙</p>
                    </div>
                </van-row>
                <van-row type="flex" justify="space-between" align="center" class="nav" @click="$router.push('/help-list?tag=外卖')">
                    <div class="item">
                        <van-row type="flex" justify="" align="center" class="top">
                            <div class="img"><img src="../assets/waimai.png" alt=""></div>
                            <div class="">
                                <p class="name">帮拿外卖</p>
                                <p class="">已帮<span>67</span></p>
                            </div>
                        </van-row>
                        <p class="text">开黑走不开？朋友来帮忙</p>
                    </div>
                </van-row>
                <van-row type="flex" justify="space-between" align="center" class="nav" @click="$router.push('/help-list?tag=跑腿')">
                    <div class="item">
                        <van-row type="flex" justify="" align="center" class="top">
                            <div class="img"><img src="../assets/paotui.png" alt=""></div>
                            <div class="">
                                <p class="name">帮忙跑腿</p>
                                <p class="">已帮<span>26</span></p>
                            </div>
                        </van-row>
                        <p class="text">校内送文件，极速抵达</p>
                    </div>
                </van-row>
                <van-row type="flex" justify="space-between" align="center" class="nav" @click="$router.push('/help-list?tag=带货')">
                    <div class="item">
                        <van-row type="flex" justify="" align="center" class="top">
                            <div class="img"><img src="../assets/daihuo.png" alt=""></div>
                            <div class="">
                                <p class="name">帮忙带货</p>
                                <p class="">已帮<span>89</span></p>
                            </div>
                        </van-row>
                        <p class="text">食堂带饭一键搞定</p>
                    </div>
                </van-row>
            </van-row>
        </div>
        <p class="title">最新发布</p>
        <GoodsList ref="goodsList" :max-page-index="1" :query-fun="queryFun" class="goods-list"/>
        <div class="characteristic">
            <Divider>互帮特色</Divider>
            <van-row type="flex" justify="space-between" align="center" class="">
                <div class="item">
                    <div><img src="../assets/kekao.png" alt=""></div>
                    <p>真实可靠</p>
                </div>
                <div class="item">
                    <div><img src="../assets/fangxin.png" alt=""></div>
                    <p>放心帮助</p>
                </div>
                <div class="item">
                    <div><img src="../assets/24h.png" alt=""></div>
                    <p>全天候服务</p>
                </div>
            </van-row>
        </div>
        <basic-footer />
        <PrivacyAgreement />
    </div>
</template>

<script>
// import AV from 'leancloud-storage'
import { Divider } from 'vant'
import PrivacyAgreement from '../components/privacy-agreement'
import GoodsList from '../components/goods-list'

export default {
    name: 'home',
    components: {
        PrivacyAgreement,
        GoodsList,
        Divider
    },
    data () {
        return {
        }
    },
    computed: {
        caseNumber () {
            const domain = document.domain
            let text = null
            switch (domain) {
            case 'www.jinshen.xyz':
            // case 'localhost':
                text = '鲁ICP备20018859号-1'
                break
            }
            console.log(text)
            return text
        },
        queryFun () {
            const fun = () => {
                return (Fun) => {
                    Fun.equalTo('state', 0)
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
    }
}
</script>
<style lang="scss" scoped>
#home {
    // background: #fff;
    .banner {
        height: 200px;
        margin-bottom: 100px;
        padding: 60px 30px 0;
        box-sizing: border-box;
        background: #F44747;
        background-image: linear-gradient(179deg, rgba(241,5,5,0.53) 0%, rgba(255,212,212,0.87) 83%);
        border-top: 1px solid #F44747;
        img {
            display: block;
            // margin: ;
            width: 100%;
            border-radius: 20px;
            // box-sizing: border-box;
            // height: 300px;
            background: #fff;
        }
    }
    .banner-2 {
        img {
            width: 100%;
        }
    }
    > .title {
        margin: 30px 30px 0;
        font-size: 32px;
        color: #333;
    }
    .list {
        margin: 0 30px;
        .title {
            margin-bottom: 30px;
            font-size: 32px;
            color: #333;
        }
        .nav {
            .item {
                width: 340px;
                margin-bottom: 20px;
                padding: 20px;
                border: 1px solid #eee;
                box-sizing: border-box;
                background: #fff;
                box-shadow: 0px 10px 10px 0px rgb(0 0 0 / 6%);
            }
            .top {
                margin-bottom: 10px;
                .img {
                    width: 90px;
                    height: 90px;
                    margin-right: 30px;
                    img {
                        max-width: 100%;
                        max-height: 100%;
                        vertical-align: middle;
                    }
                }
            }
            .text {
                text-align: center;
            }
            .name {
                font-size: 28px;
                color: #333;
                &~p {
                    font-size: 24px;
                    color: #888;
                    span {
                        color: #F44747;
                    }
                }
            }
            .text {
                font-size: 24px;
                color: #888;
            }
        }
    }
    .characteristic {
        padding: 0 100px;
        font-size: 24px;
        color: #1296db;
        text-align: center;
        img {
            max-width: 70px;
        }
    }
}
</style>x
