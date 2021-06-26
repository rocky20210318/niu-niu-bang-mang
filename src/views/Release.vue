<template>
    <div id="release">
        <Form @submit="onSubmit">
            <div class="box">
                <Field v-model="form.title" label="标题" placeholder="标题" maxlength="10" :rules="[{ required: true, message: '请填标题' }]" />
                <Field v-model="form.content" type="textarea" label="帮忙内容" placeholder="需要帮忙的具体内容" maxlength="200" autosize show-word-limit :rules="[{ required: true, message: '请填需要帮忙的内容' }]" />
            </div>
            <div class="box">
                <Field v-model="form.address" type="textarea" label="取货地址" placeholder="详细取货地址"  />
            </div>
            <div class="box">
                <Field v-model="form.receivingAddress" type="textarea" autosize label="收货货地址" placeholder="详细取货地址">
                    <template #button>
                        <Button native-type="button" size="small" color="#1296db" type="primary" to="/address-list">常用地址</Button>
                    </template>
                </Field>
                <Field v-model="form.name" label="联系人" placeholder="联系人姓名"  />
                <Field v-model="form.phone" label="联系电话" placeholder="联系电话" :rules="[{ required: true, message: '请填联系电话' }]"/>
            </div>
            <div class="box">
                <Field v-model="form.money" type="number" label="报酬金额" placeholder="报酬金额" :rules="[{ required: true, message: '请填报酬金额' }]" />
            </div>
            <div class="box">
                <Field v-model="endTime" label="结束时间" readonly placeholder="未设置结束时间，默认长期7天有效" @click="isShow = true" />
                <Field name="radio" label="分类">
                    <template #input>
                        <RadioGroup v-model="form.tag" direction="horizontal">
                            <Radio name="快递" checked-color="#ff202c">拿快递</Radio>
                            <Radio name="外卖" checked-color="#ff202c">拿外卖</Radio>
                            <Radio name="跑腿" checked-color="#ff202c">跑腿</Radio>
                            <Radio name="带货" checked-color="#ff202c">带货</Radio>
                            <Radio name="其他" checked-color="#ff202c">其他</Radio>
                        </RadioGroup>
                    </template>
                </Field>
            </div>
            <Button class="button" type="block" color="linear-gradient(135deg, #ffb990 0%, #ff3241 100%)" round :loading="loading" native-type="submit">发布</Button>
        </Form>
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
        <Popup v-model="isShow" position="bottom">
            <DatetimePicker :min-date="minDate" :max-date="maxData" title="选择结束时间" @confirm="confirm" @cancel="isShow = false" />
        </Popup>
        <basic-footer />
    </div>
</template>

<script>
import { Form, Field, Button, RadioGroup, Radio, DatetimePicker, Popup, Dialog } from 'vant'
import { format } from '../utils/index'
import { getAddress } from '../services'
import AV from 'leancloud-storage'

export default {
    name: 'release',
    components: {
        Form,
        Field,
        Button,
        RadioGroup,
        Radio,
        DatetimePicker,
        Popup
    },
    data () {
        let timestamp = new Date().getTime()
        timestamp = timestamp + 12 * 60 * 60 * 1000
        const dateAfter = new Date(timestamp)
        return {
            minDate: new Date(),
            maxData: new Date(2021, 11, 31),
            isShow: false,
            loading: false,
            endTime: format(dateAfter, 'YYYY-MM-dd HH:mm'),
            addressIndex: this.$route.query.index || 0,
            form: {
                title: '',
                content: '',
                address: '',
                phone: AV.User.current().get('mobilePhoneNumber'),
                name: '',
                money: 1,
                endTime: dateAfter,
                tag: '快递',
                receivingAddress: '',
                createUser: AV.User.current()
            }
        }
    },
    computed: {
    },
    async created () {
        this.getAddress()
    },
    activated () {
        // console.log(1)
        this.addressIndex = this.$route.query.index || 0
        // console.log(this.addressIndex)
        this.getAddress()
    },
    mounted () {
    },
    methods: {
        async onSubmit () {
            this.loading = true
            const Add = AV.Object.extend('HelpList')
            const add = new Add()
            for (const key in this.form) {
                add.set(key, key === 'money' ? Number(this.form[key]) : this.form[key])
                // console.log(this.form[key])
            }
            await add.save()
            Dialog({
                title: '提示',
                message: '发布成功！'
            }).then(() => {
                this.$router.push('/order-list?showType=1')
            })
            this.loading = false
        },
        confirm (date) {
            this.form.endTime = date
            this.endTime = format(date, 'YYYY-MM-dd HH:mm')
            this.isShow = false
        },
        async getAddress () {
            const listData = await getAddress()
            if (listData.length !== 0) {
                const data = listData[Number(this.addressIndex)]
                this.form.receivingAddress = `${data.province} ${data.city} ${data.county} ${data.addressDetail}`
                this.form.name = data.name
                this.form.phone = data.tel
            }
            // console.log(this.list, listData, typeof listData)
        }
    }
}
</script>
<style lang="scss">

</style>
<style lang="scss" scoped>
#release {
    padding: 30px;
    .box {
        border-radius: 10px;
        margin-bottom: 30px;
        overflow: hidden;
        box-shadow: 0px 8px 20px 0px rgba(0, 0, 0, 0.1)
    }
    .van-radio {
        margin-bottom: 14px;
    }
    .button {
        margin-top: 60px;
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
