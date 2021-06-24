<template>
    <div id="details">
        <van-nav-bar fixed left-arrow @click-left="$router.go(-1)" placeholder title="帮忙详情" />
        <p class="title">{{ form.title }}</p>
        <div class="box">
            <van-row type="flex" justify="space-between" align="center" class="createdAt-tag" >
                <p class="createdAt">发布时间：{{ createdAt }}</p>
                <p class="tag">{{ form.tag }}</p>
            </van-row>
            <p class="content">{{ form.content }}</p>
            <van-row type="flex" justify="space-between" align="center" class="endTime-money" >
                <p class="endTime">结束时间：{{ endTime }}</p>
                <p class="money">¥<span>{{ form.money }}</span>酬劳</p>
            </van-row>
        </div>
        <div class="box address">
            取货地址：{{ form.address }}
            <div v-if="form.state === 4" class="complete"><img src="../assets/complete.png" alt=""></div>
        </div>
        <div class="box">
            <p class="receivingAddress">收货地址：{{ form.address }}</p>
            <p class="name">收货人：{{ form.name }}</p>
            <p class="phone">联系电话：{{ form.phone }}</p>
        </div>
        <Button v-if="userData.id !== form.createUserId && form.state === 0" type="block" color="linear-gradient(135deg, #ffb990 0%, #ff3241 100%)" @click="receive" :loading="loading" round class="button">去帮忙</Button>
        <Button v-if="userData.id === form.createUserId && form.state === 1" type="block" color="linear-gradient(135deg, #ffb990 0%, #ff3241 100%)" @click="complete" :loading="loading" round class="button">任务完成</Button>
        <Button v-if="userData.id !== form.collectUsersId &&form.state !== 0" type="block" color="linear-gradient(135deg, #ffb990 0%, #ff3241 100%)" disabled round class="button">已被领取</Button>
        <div class="characteristic">
            <!-- <Divider>互帮特色</Divider> -->
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
        <!-- <basic-footer /> -->
    </div>
</template>

<script>
import { Button, Dialog } from 'vant'
import { format } from '../utils/index'
import AV from 'leancloud-storage'

export default {
    name: 'details-1',
    components: {
        Button
    },
    data () {
        return {
            id: this.$route.params.id,
            loading: false,
            endTime: '',
            createdAt: '',
            userData: AV.User.current(),
            form: {}
        }
    },
    computed: {
    },
    async created () {
        this.getDetails()
    },
    mounted () {
    },
    methods: {
        confirm (date) {
            // this.form.endTime = date
            return format(date, 'YYYY-MM-dd HH:mm')
            // this.isShow = false
        },
        async getDetails () {
            const Details = new AV.Query('HelpList')
            const details = await Details.get(this.id)
            console.log(details)
            this.form = {
                createdAt: details.createdAt,
                createUserId: details.get('createUser').id,
                collectUsersId: details.get('collectUsers') ? details.get('collectUsers').id : null,
                ...details.attributes
            }
            this.endTime = this.confirm(details.get('endTime'))
            this.createdAt = this.confirm(details.createdAt)
        },
        async receive () {
            this.loading = true
            const Details = new AV.Query('HelpList')
            const details = await Details.get(this.id)
            details.set('collectUsers', AV.User.current())
            details.set('state', 1)
            await details.save()
            this.loading = false
            this.getDetails()
        },
        async complete () {
            Dialog.confirm({
                title: '确认',
                message: '是否确认完成任务？'
            }).then(async () => {
                this.loading = true
                const Details = new AV.Query('HelpList')
                const details = await Details.get(this.id)
                details.set('state', 4)
                await details.save()
                this.loading = false
                this.getDetails()
            })
        }
    }
}
</script>
<style lang="scss">

</style>
<style lang="scss" scoped>
#details {
    // padding-bottom: 60px;
    // background: #fff;
    // min-height: 100%;
    padding: 0 30px 60px;
    // padding: 0 30px;
    .box {
        position: relative;
        margin-bottom: 30px;
        padding: 40px;
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 8px 20px 0px rgba(0, 0, 0, 0.1)
    }
    .title {
        margin: 32px 0;
        font-size: 32px;
        color: #333;
        text-align: center;
    }
    .createdAt-tag {
        margin-bottom: 8px;
        font-size: 26px;
        color: #888;
    }
    .content {
        margin-bottom: 10px;
        font-size: 28px;
        color: #333;
        line-height: 1.5;
    }
    .endTime-money {
        // margin-bottom: 20px;
        font-size: 26px;
        color: #888;
        .money {
            color: #333;
            span {
                margin-right: 4px;
                font-size: 38px;
                color: #ff3241;
            }
        }
    }
    .address,
    .receivingAddress,
    .name,
    .phone {
        margin-bottom: 20px;
        line-height: 1;
        font-size: 28px;
        color: #333;
    }
    .phone {
        margin-bottom: 0;
    }
    .button {
        margin: 60px 0;
    }
    .complete {
        position: absolute;
        z-index: 2;
        top: -60px;
        right: 0px;
    }
    .characteristic {
        padding: 60px 30px;
        font-size: 32px;
        color: #1296db;
        text-align: center;
        img {
            max-width: 90px;
        }
    }
}
</style>
