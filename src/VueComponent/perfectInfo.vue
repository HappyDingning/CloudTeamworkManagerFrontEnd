<template>
    <div class="container" id="mainFunc">
        <div class="row no-gutters justify-content-md-center" style="height: 100%">
            <div class="col-5 align-self-center">
                <div class="text-right clearfix">
                    <div class="text-uppercase font-weight-light" style="letter-spacing: 4px; color: #536DFE; font-size: 48px">
                        perfect&nbsp;info
                    </div>
                    <div style="letter-spacing: 4px; font-size: 22px; color: #333333">补全你的个人信息
                    </div>
                    <hr noshade=true style="width: 17%; height: 1px; background-color: #536DFE; float: right;">
                </div>
                <div class="row justify-content-center no-gutters p-0 mb-5 bg-white" style="border-radius: 0px; box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16)" id="mainFunc">
                    <div class="col-10" style="margin-top: 10%; margin-bottom: 10%">
                        <transition :name="basic_info_part_one_name">
                            <form v-if="basic_info_part_one" class="clearfix">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的姓名（确定后无法修改）</label>
                                <input v-model="name" class="form-control" placeholder="示例: 王美美" style="margin-bottom: 1rem; border-radius: 0px">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请选择你的生日（确定后无法修改）</label>
                                <input v-model="birthday" type="date" class="form-control" style="margin-bottom: 1rem; border-radius: 0px">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请选择你的性别（确定后无法修改）</label>
                                <select v-model="sex" class="form-control" style="margin-bottom: 1rem; border-radius: 0px">
                                    <option selected value="">请选择</option>
                                    <option value='m'>男</option>
                                    <option value='f'>女</option>
                                </select>
                            </form>
                        </transition>
                        <transition :name="basic_info_part_two_name">
                            <form v-if="basic_info_part_two" class="clearfix">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的学号（确定后无法修改）</label>
                                <input v-model="student_id" type="text" class="form-control"
                                    placeholder="示例: 2015006797" style="margin-bottom: 1rem; border-radius: 0px">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的云ID（确定后无法修改）</label>
                                <input v-model="cloud_id" type="text" class="form-control" placeholder="示例: 1234567" style="margin-bottom: 1rem; border-radius: 0px">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请选择你的方向（确定后无法修改）</label>
                                <select v-model="major" class="form-control" style="margin-bottom: 1rem; border-radius: 0px">
                                    <option selected value="">请选择</option>
                                    <option value='M'>机电创新团队</option>
                                    <option value='P'>Python</option>
                                    <option value='N'>Node.js</option>
                                    <option value='J'>Java</option>
                                    <option value='F'>前端</option>
                                    <option value='D'>设计</option>
                                </select>
                            </form>
                        </transition>
                        <transition :name="extend_info_part_one_name">
                            <form v-if="extend_info_part_one" class="clearfix">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的邮箱</label>
                                <input v-model="email" type="text" class="form-control"
                                    placeholder="示例: someone@example.com" style="margin-bottom: 1rem; border-radius: 0px">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的宿舍</label>
                                <input v-model="room" type="text" class="form-control" placeholder="示例: 2#227" style="margin-bottom: 1rem; border-radius: 0px">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的家庭地址（可选）</label>
                                <input v-model="home_address" type="text" class="form-control" placeholder="示例:山西省太原市小店区" style="margin-bottom: 1rem; border-radius: 0px">
                            </form>
                        </transition>
                        <transition :name="extend_info_part_two_name">
                            <form v-if="extend_info_part_two" class="clearfix">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的监护人的电话（可选）</label>
                                <input v-model="guardian_phone" type="text" class="form-control"
                                    placeholder="示例: 15698310000" style="margin-bottom: 1rem; border-radius: 0px">
                                <label style="font-size: 14px; margin: 0rem 0rem 0.5rem 0rem; color: #333333">请输入你的个人介绍（可选）</label>
                                <textarea v-model="introduction" type="text" class="form-control"
                                    placeholder="我是一个活泼开朗的..."
                                    style="height: 124px; border: 1px solid #ced4da; margin-bottom: 1rem; border-radius: 0px"></textarea>
                            </form>
                        </transition>
                        <transition :name="extend_info_avatar_name">
                            <form v-if="extend_info_avatar" class="clearfix">
                                <div style="text-align: center; height: 169px">
                                    <img :src="src" style="width: 40%" :style="{height: avatar_height}"
                                        class="rounded-circle" id="avatar">
                                </div>
                                <label for="file" class="btn btn-primary col-6 offset-3"
                                    style="cursor: pointer; border-radius: 0px">请选择头像（可选）</label>
                                <input ref="referenceUpload" type="file" name="avatar" accept=".jpg" v-on:change="show_avatar($event)"
                                    id="file" style="height: 0px; opacity: 0;">
                            </form>
                        </transition>
                        <div class="tip" style="height: 14px; margin-top: 1.5rem; font-size: 14px; color: #E86263">
                            {{  tip  }}
                        </div>
                        <input v-if="!basic_info_part_one" @click="previous" type="button" class="btn btn-primary col-2"
                            value="<" style="float: left; font-size: 18px; margin-top: 3.6rem; border-radius: 0px">
                        <input @click="next" type="button" class="btn btn-primary col-6 offset-4" :value="right_button"
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
    /* 这是next动画 */
    .basic_info_part_one_next-enter {
        transform: translateX(10%);
        opacity: 0;
    }

    .basic_info_part_one_next-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .basic_info_part_one_next-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_one_next-leave-to {
        transform: translateX(-10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_one_next-enter-active,
    .basic_info_part_one_next-leave-active {
        transition: all .5s ease;
    }

    .basic_info_part_two_next-enter {
        transform: translateX(10%);
        opacity: 0;
    }

    .basic_info_part_two_next-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .basic_info_part_two_next-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_two_next-leave-to {
        transform: translateX(-10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_two_next-enter-active,
    .basic_info_part_two_next-leave-active {
        transition: all .5s ease;
    }

    .extend_info_part_one_next-enter {
        transform: translateX(10%);
        opacity: 0;
    }

    .extend_info_part_one_next-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .extend_info_part_one_next-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_one_next-leave-to {
        transform: translateX(-10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_one_next-enter-active,
    .extend_info_part_one_next-leave-active {
        transition: all .5s ease;
    }

    .extend_info_part_two_next-enter {
        transform: translateX(10%);
        opacity: 0;
    }

    .extend_info_part_two_next-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .extend_info_part_two_next-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_two_next-leave-to {
        transform: translateX(-10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_two_next-enter-active,
    .extend_info_part_two_next-leave-active {
        transition: all .5s ease;
    }

    .extend_info_avatar_next-enter {
        transform: translateX(10%);
        opacity: 0;
    }

    .extend_info_avatar_next-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .extend_info_avatar_next-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .extend_info_avatar_next-leave-to {
        transform: translateX(10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .extend_info_avatar_next-enter-active,
    .extend_info_avatar_next-leave-active {
        transition: all .5s ease;
    }

    /* 这是previous动画 */
    .basic_info_part_one_previous-enter {
        transform: translateX(-10%);
        opacity: 0;
    }

    .basic_info_part_one_previous-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .basic_info_part_one_previous-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_one_previous-leave-to {
        transform: translateX(-10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_one_previous-enter-active,
    .basic_info_part_one_previous-leave-active {
        transition: all .5s ease;
    }

    .basic_info_part_two_previous-enter {
        transform: translateX(-10%);
        opacity: 0;
    }

    .basic_info_part_two_previous-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .basic_info_part_two_previous-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_two_previous-leave-to {
        transform: translateX(10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .basic_info_part_two_previous-enter-active,
    .basic_info_part_two_previous-leave-active {
        transition: all .5s ease;
    }

    .extend_info_part_one_previous-enter {
        transform: translateX(-10%);
        opacity: 0;
    }

    .extend_info_part_one_previous-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .extend_info_part_one_previous-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_one_previous-leave-to {
        transform: translateX(10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_one_previous-enter-active,
    .extend_info_part_one_previous-leave-active {
        transition: all .5s ease;
    }

    .extend_info_part_two_previous-enter {
        transform: translateX(-10%);
        opacity: 0;
    }

    .extend_info_part_two_previous-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .extend_info_part_two_previous-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_two_previous-leave-to {
        transform: translateX(10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .extend_info_part_two_previous-enter-active,
    .extend_info_part_two_previous-leave-active {
        transition: all .5s ease;
    }

    .extend_info_avatar_previous-enter {
        transform: translateX(-10%);
        opacity: 0;
    }

    .extend_info_avatar_previous-enter-to {
        transform: translateX(0%);
        opacity: 1;
    }

    .extend_info_avatar_previous-leave {
        transform: translateX(0%);
        opacity: 1;
        position: absolute;
        top: 0px;
    }

    .extend_info_avatar_previous-leave-to {
        transform: translateX(10%);
        opacity: 0;
        position: absolute;
        top: 0px;
    }

    .extend_info_avatar_previous-enter-active,
    .extend_info_avatar_previous-leave-active {
        transition: all .5s ease;
    }
</style>

<script>
    export default {
        data() {
            return {
                basic_info_part_one: true,
                basic_info_part_two: false,
                extend_info_part_one: false,
                extend_info_part_two: false,
                extend_info_avatar: false,
                basic_info_part_one_name: "basic_info_part_one_next",
                basic_info_part_two_name: "basic_info_part_two_next",
                extend_info_part_one_name: "extend_info_part_one_next",
                extend_info_part_two_name: "extend_info_part_two_next",
                extend_info_avatar_name: "extend_info_avatar_next",
                right_button: '下一步',
                name: "",
                birthday: "",
                sex: "",
                student_id: "",
                cloud_id: "",
                major: "",
                email: "",
                room: "",
                home_address: "",
                guardian_phone: "",
                introduction: "",
                avatar: "",
                avatar_height: "auto",
                src: "/static/pic/组 18.png",
                tip: "",
            }
        },
        props: ['globle_props'],
        methods: {
            mySwitch: function(target) {
                this.$emit('switch', target);
            },
            show_avatar: function (event) {
                this.avatar = event.target.files[0];
                this.src = window.URL.createObjectURL(this.avatar);
                this.$refs.referenceUpload.value = null;
                this.avatar_height = document.getElementById("avatar").scrollWidth + 'px';
            },
            submit: function () {
                if (this.avatar){
                    let formData = new FormData();
                    formData.append('avatar', this.avatar);
                    this.$http.post('/file/avatar/1/', formData, { headers: { 'Content-Type': 'multipart/form-data' } }).then(result => {
                    if (result.body.status == 200) {
                        ;
                    }
                    else {
                        this.tip = result.body.tip;
                        return;
                    }
                })
                }
                this.$http.post('/account/perfect_information/', { "name": this.name, "birthday": this.birthday, "sex": this.sex, "student_id": this.student_id, "grade": this.student_id.substr(0, 4), "cloud_id": this.cloud_id, "major": this.major, "email": this.email, "room": this.room, "home_address": this.home_address, "guardian_phone": this.guardian_phone, "introduction": this.introduction }, { emulateJSON: true }).then(result => {
                    if (result.body.status == 200) {
                        this.globle_props.perfected_info = true;
                        this.$emit('fresh_user_info', '');
                        this.mySwitch(['space', null]);
                    }
                    else {
                        this.tip = result.body.tip;
                        return;
                    }
                })
            },
            previous: function () {
                if (this.extend_info_avatar) {
                    this.extend_info_avatar_name = "extend_info_avatar_previous";
                    this.extend_info_part_two_name = "extend_info_part_two_previous";
                    this.extend_info_avatar = false;
                    this.extend_info_part_two = true;
                    this.right_button = "下一步";
                }
                else if (this.extend_info_part_two) {
                    this.extend_info_part_two_name = "extend_info_part_two_previous";
                    this.extend_info_part_one_name = "extend_info_part_one_previous";
                    this.extend_info_part_two = false;
                    this.extend_info_part_one = true;
                }
                else if (this.extend_info_part_one) {
                    this.extend_info_part_one_name = "extend_info_part_one_previous";
                    this.basic_info_part_two_name = "basic_info_part_two_previous";
                    this.extend_info_part_one = false;
                    this.basic_info_part_two = true;
                }
                else {
                    this.basic_info_part_two_name = "basic_info_part_two_previous";
                    this.basic_info_part_one_name = "basic_info_part_one_previous";
                    this.basic_info_part_two = false;
                    this.basic_info_part_one = true;
                }
            },
            next: function () {
                if (this.basic_info_part_one) {
                    if (this.check_basic_info_part_one()) {
                        this.basic_info_part_one_name = "basic_info_part_one_next";
                        this.basic_info_part_two_name = "basic_info_part_two_next";
                        this.basic_info_part_one = false;
                        this.basic_info_part_two = true;
                    }
                }
                else if (this.basic_info_part_two) {
                    if (this.check_basic_info_part_two()) {
                        this.basic_info_part_two_name = "basic_info_part_two_next";
                        this.extend_info_part_one_name = "extend_info_part_one_next";
                        this.basic_info_part_two = false;
                        this.extend_info_part_one = true;
                    }
                }
                else if (this.extend_info_part_one) {
                    this.extend_info_part_one_name = "extend_info_part_one_next";
                    this.extend_info_part_two_name = "extend_info_part_two_next";
                    this.extend_info_part_one = false;
                    this.extend_info_part_two = true;
                }
                else if (this.extend_info_part_two) {
                    if (this.check_extend_info_part_two()) {
                        this.extend_info_avatar_name = "extend_info_avatar_next";
                        this.extend_info_part_two_name = "extend_info_part_two_next";
                        this.extend_info_part_two = false;
                        this.extend_info_avatar = true;
                        this.right_button = "提交";
                    }
                }
                else {
                    this.submit();
                }
            },

            // 表单验证部分
            check_basic_info_part_one: function () {
                if (this.name.length < 2 || this.name.length > 5) {
                    this.tip = "姓名不正确"
                    return false;
                }
                if (!this.birthday) {
                    this.tip = "请选择生日"
                    return false;
                }
                if (!this.sex) {
                    this.tip = "请选择性别"
                    return false;
                }
                this.tip = ""
                return true;
            },
            check_basic_info_part_two: function () {
                if (this.student_id.length != 10) {
                    this.tip = "学号不正确"
                    return false;
                }
                if (!this.cloud_id) {
                    this.tip = "请输入云顶号"
                    return false;
                }
                if (!this.major) {
                    this.tip = "请选择方向"
                    return false;
                }
                this.tip = ""
                return true;
            },
            check_extend_info_part_two: function () {
                if (this.guardian_phone.length != 11 && this.guardian_phone.length != 0) {
                    this.tip = "监护人电话不正确"
                    return false;
                }
                this.tip = ""
                return true
            }
        }
    };
</script>
