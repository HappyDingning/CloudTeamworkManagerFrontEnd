<template>
    <div class="container" id="mainFunc">
        <div class="row no-gutters justify-content-md-center" style="height: 100%">
            <div class="col-5 align-self-center">
                <div class="text-right clearfix">
                    <div class="text-uppercase font-weight-light"
                        style="letter-spacing: 4px; color: #536DFE; font-size: 48px">password
                    </div>
                    <div style="letter-spacing: 4px; font-size: 22px; color: #333333">更改密码
                    </div>
                    <hr noshade=true style="width: 17%; height: 1px; background-color: #536DFE; float: right;">
                </div>
                <div class="row justify-content-center no-gutters p-0 mb-5 bg-white"
                    style="border-radius: 0px; box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16)" id="mainFunc">
                    <div class="col-10" style="margin-top: 10%; margin-bottom: 10%">
                        <form action="" method="POST">
                            <label :style="{visibility: show_phone_number_label ? 'visible' : 'hidden'}"
                                style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入您的手机号</label>
                            <input v-model="phone_number" type="text" autocomplete=off style="margin-bottom: 1rem; border-radius: 0px"
                                class="form-control" :placeholder="show_phone_number_label? '' : '请输入您的手机号'"
                                @focus="show_phone_number_label = !show_phone_number_label"
                                @blur="show_phone_number_label = !show_phone_number_label">
                            <label :style="{visibility: show_old_password_label ? 'visible' : 'hidden'}"
                                style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入您的旧密码</label>
                            <input v-model="old_password" type="password"
                                style="margin-bottom: 1rem; border-radius: 0px" class="form-control"
                                :placeholder="show_old_password_label? '' : '请输入您的旧密码'"
                                @focus="show_old_password_label = !show_old_password_label"
                                @blur="show_old_password_label = !show_old_password_label">
                            <label :style="{visibility: show_password_label ? 'visible' : 'hidden'}"
                                style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入您的新密码</label>
                            <input v-model="password" type="password" style="margin-bottom: 1rem; border-radius: 0px"
                                class="form-control" :placeholder="show_password_label? '' : '请输入您的新密码'"
                                @focus="show_password_label = !show_password_label"
                                @blur="show_password_label = !show_password_label">
                            <label :style="{visibility: show_re_password_label ? 'visible' : 'hidden'}"
                                style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请再次输入您的新密码</label>
                            <input v-model="re_password" type="password" style="margin-bottom: 1rem; border-radius: 0px"
                                class="form-control" :placeholder="show_re_password_label? '' : '请再次输入您的新密码'"
                                @focus="show_re_password_label = !show_re_password_label"
                                @blur="show_re_password_label = !show_re_password_label">
                            <div>
                                <label :style="{visibility: show_picode_label ? 'visible' : 'hidden'}"
                                    style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入图形验证码</label>
                                <input v-model="picode" type="text" autocomplete=off style="margin-bottom: 1rem; border-radius: 0px;"
                                    class="form-control" :placeholder="show_picode_label? '' : '请输入图形验证码'"
                                    @focus="show_picode_label = !show_picode_label"
                                    @blur="show_picode_label = !show_picode_label" @keyon.enter="submit">
                                <img @click="reload_picode" :src="imgsrc"
                                    style="float: right; transform: translateY(-55px); height: 37.84px; width: 30%; border-radius: 0rem">
                            </div>
                        </form>
                        <div class="tip" style="height: 14px; margin-top: 0.5rem; font-size: 14px; color: #E86263">{{  tip  }}</div>
                        <input @click="submit" type="button" class="btn btn-primary col-6 offset-6" value="提交"
                            style="float: right; font-size: 18px; margin-top: 3.6rem; border-radius: 0px">
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
            return {
                tip: "",
                phone_number: "",
                old_password: "",
                password: "",
                re_password: "",
                picode: "",
                imgsrc: "/file/picode/",
                show_phone_number_label: false,
                show_old_password_label: false,
                show_password_label: false,
                show_re_password_label: false,
                show_picode_label: false,
            }
        },
        methods: {
            submit: function () {
                if (this.submit_check()) {
                    this.$http.post('/account/set_password/', { 'phone_number': this.phone_number, 'new_password': this.password, 'old_password': this.old_password, 'picode': this.picode }, { emulateJSON: true }).then(result => {
                        if (result.body.status == 200) {
                            this.mySwitch(['login', null]);
                        }
                        else {
                            this.tip = result.body.tip;
                            this.reload_picode();
                        }
                    })
                }
            },
            mySwitch: function(target) {
                this.$emit('switch', target)
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
                return true;
            },
            reload_picode: function () {
                this.imgsrc = this.imgsrc + Math.ceil(10)
            }
        },
    };
</script>
