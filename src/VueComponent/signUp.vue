<template>
    <div class="container" id="mainFunc">
        <div class="row no-gutters justify-content-md-center" style="height: 100%">
            <div class="col-5 align-self-center">
                <div class="text-right clearfix">
                    <div class="letter-spacing: 4px; text-uppercase font-weight-light"
                        style="color: #536DFE; font-size: 48px">sign&nbsp;up
                    </div>
                    <div style="letter-spacing: 4px; font-size: 22px; color: #333333">注册您的云顶账号
                    </div>
                    <hr noshade=true style="width: 17%; height: 1px; background-color: #536DFE; float: right;">
                </div>
                <div class="row justify-content-center no-gutters p-0 mb-5 bg-white"
                    style="border-radius: 0px; box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16)">
                    <div class="col-10" style="margin-top: 4.5rem; margin-bottom: 2rem">
                        <form action="" method="POST" class="clearfix">
                            <label :style="{visibility: show_phone_number_label ? 'visible' : 'hidden'}"
                                style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入您的手机号</label>
                            <input v-model="phone_number" type="text" autocomplete=off class="form-control"
                                style="margin-bottom: 1rem; border-radius: 0px"
                                :placeholder="show_phone_number_label? '' : '请输入您的手机号'"
                                @focus="show_phone_number_label = !show_phone_number_label"
                                @blur="show_phone_number_label = !show_phone_number_label">
                            <div>
                                <label :style="{visibility: show_picode_label ? 'visible' : 'hidden'}"
                                    style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入图形验证码</label>
                                <input v-model="picode" type="text" class="form-control" autocomplete=off
                                    style="margin-bottom: 1rem; border-radius: 0px"
                                    :placeholder="show_picode_label? '' : '请输入图形验证码'"
                                    @focus="show_picode_label = !show_picode_label"
                                    @blur="show_picode_label = !show_picode_label">
                                <img @click="reload_picode" :src="imgsrc"
                                    style="float: right; transform: translateY(-55px); height: 37.84px; width: 30%">
                            </div>
                            <div>
                                <label :style="{visibility: show_msgcode_label ? 'visible' : 'hidden'}"
                                    style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入您的短信验证码</label>
                                <input v-model="msgcode" type="text" class="form-control" autocomplete=off
                                    style="margin-bottom: 1rem; border-radius: 0px"
                                    :placeholder="show_msgcode_label? '' : '请输入您的短信验证码'"
                                    @focus="show_msgcode_label = !show_msgcode_label"
                                    @blur="show_msgcode_label = !show_msgcode_label">
                                <button @click="send_msgcode" class="btn btn-primary" type="button"
                                    style="float: right; transform: translateY(-55px); border-radius: 0px; width: 30%">
                                    <span>点击获取</span>
                                </button>
                            </div>
                            <label :style="{visibility: show_password_label ? 'visible' : 'hidden'}"
                                style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入您的密码</label>
                            <input v-model="password" type="password" class="form-control"
                                style="margin-bottom: 1rem; border-radius: 0px"
                                :placeholder="show_password_label? '' : '请输入您的密码'"
                                @focus="show_password_label = !show_password_label"
                                @blur="show_password_label = !show_password_label">
                            <label :style="{visibility: show_re_password_label ? 'visible' : 'hidden'}"
                                style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请再次输入您的密码</label>
                            <input v-model="re_password" type="password" class="form-control"
                                style="margin-bottom: 1rem; border-radius: 0px"
                                :placeholder="show_re_password_label? '' : '请再次输入您的密码'"
                                @focus="show_re_password_label = !show_re_password_label"
                                @blur="show_re_password_label = !show_re_password_label" @keyon.enter="submit">
                        </form>
                        <div class="tip" style="height: 14px; margin-top: 0.5rem; font-size: 14px; color: #E86263">
                            {{  tip  }} </div>
                        <input @click="submit" type="button" class="btn btn-primary col-6 offset-6" value="注册"
                            style="float: right; font-size: 18px; margin-top: 3.6rem; border-radius: 0px">
                    </div>
                    <div class="text-center col-12 align-self-end rounded-bottom align-middle"
                        style="height: 50px; background-color: rgba(130, 178, 255, 0.2); font-size: 16px">
                        <div style="position: relative; top: 50%; transform: translateY(-50%); color: #999999">已有账号？<a
                                href="/account/login" v-on:click.prevent="mySwitch(['signIn', null])">点击登录</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    #mainFunc{
        height: 100%;
    }
</style>

<script>
    export default {
        data() {
            return{
                tip: "",
                phone_number: "",
                password: "",
                msgcode: "",
                picode: "",
                re_password: "",
                imgsrc: "/file/picode/",
                show_phone_number_label: false,
                show_picode_label: false,
                show_msgcode_label: false,
                show_password_label: false,
                show_re_password_label: false,
            }
        },
        methods: {
            submit: function () {
                if (this.submit_check()) {
                    this.$http.post('/account/register/', { 'phone_number': this.phone_number, 'password': this.password, 'msgcode': this.msgcode }, { emulateJSON: true }).then(result => {
                        if (result.body.status == 200) {
                            this.mySwitch(['signIn', null]);
                        }
                        else {
                            this.tip = result.body.tip;
                        }
                    })
                }
            },
            send_msgcode: function () {
                if (this.send_msgcode_check()) {
                    this.$http.post('/account/sendmsgcode/', { 'phone_number': this.phone_number, 'picode': this.picode }, { emulateJSON: true }).then(result => {
                        this.tip = result.body.tip;
                    })
                }
            },
            submit_check: function () {
                if (this.phone_number.length < 11) {
                    this.tip = "手机号码不正确";
                    return false;
                }
                if (this.re_password != this.password) {
                    this.tip = "两次输入的密码不一致";
                    return false;
                }
                if (this.password.length < 6) {
                    this.tip = "密码过短，需要6位以上";
                    return false;
                }
                if (this.msgcode.length < 1) {
                    this.tip = "需要输入短信验证码";
                    return false;
                }
                if (this.msgcode.length != 4) {
                    this.tip = "短信验证码不正确";
                    return false;
                }
                return true;
            },
            send_msgcode_check: function () {
                if (this.phone_number.length < 11) {
                    this.tip = "手机号码不正确";
                    return false;
                }
                if (this.picode.length < 1) {
                    this.tip = "需要输入图片验证码";
                    return false;
                }
                if (this.picode.length != 4) {
                    this.tip = "图片验证码不正确";
                    return false;
                }
                return true;
            },
            reload_picode: function () {
                this.imgsrc = this.imgsrc + Math.ceil(10)
            },
            mySwitch: function(target) {
                this.$emit('switch', target)
            }
        },
    }
</script>