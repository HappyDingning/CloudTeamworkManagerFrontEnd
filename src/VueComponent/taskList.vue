<template>
    <div class="container" id="mainFunc">
        <div class="row no-gutters">
            <div class="col-10 offset-1">
                <div class="card" style="border: 0rem">
                    <div class="row no-gutters">
                        <div class="avatar col-2 offset-1">
                            <a href="/account/space/" v-on:click.prevent="mySwitch(['space', null])">
                                <div
                                    style="cursor: pointer; text-align: center; display: none; width: 100%; height: 6rem; background-color: rgba(0, 0, 0, 0.3)">
                                    <div style="color: #ffffff; line-height: 6rem;">点击打开个人主页</div>
                                </div>
                                <img :src='"/file/avatar/" + globle_props.randint' style="width: 100%; height: 6rem">
                            </a>
                        </div>
                        <div class="col-3 offset-1" style="color: #666666; font-size: 1.3rem; line-height: 6rem">
                            {{ globle_props.name }}
                        </div>
                        <div class="add col-3 offset-1" style="text-align: center; cursor: pointer;" v-if="globle_props.is_creater">
                            <a href="/task/create_task/"
                                style="display: none; line-height: 6rem; text-decoration: none; color: #ffffff" v-on:click.prevent="mySwitch(['createTask', null])">发布新任务
                            </a>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <div class="row no-gutters">
                        <div class="clearfix offset-1" style="width: 100%">
                            <div class="title">
                                <div class="text-uppercase lead font-weight-bold title_en">
                                    our&nbsp;task
                                </div>
                                <div class="title_zh">
                                    任务列表
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="row no-gutters">
                        <div class="offset-1">
                            <ul class="nav nav-pills mb-3" id="pills-tab" role="tablist">
                                <li class="nav-item">
                                    <a class="nav-link active" id="pills-all-tab" data-toggle="pill" href="#pills-all"
                                        role="tab" aria-controls="pills-all" aria-selected="true" style="border-radius: 0px;">全部任务</a>
                                </li>
                                <li class="nav-item">
                                    <a class="nav-link" id="pills-ongoing-tab" data-toggle="pill" href="#pills-ongoing"
                                        role="tab" aria-controls="pills-ongoing" aria-selected="false" style="border-radius: 0px;">正在进行</a>
                                </li>
                                <li class="nav-item">
                                    <a class="nav-link" id="pills-pause-tab" data-toggle="pill" href="#pills-pause"
                                        role="tab" aria-controls="pills-pause" aria-selected="false" style="border-radius: 0px;">已暂停</a>
                                </li>
                                <li class="nav-item">
                                    <a class="nav-link" id="pills-finished-tab" data-toggle="pill"
                                        href="#pills-finished" role="tab" aria-controls="pills-finished"
                                        aria-selected="false" style="border-radius: 0px;">已结束</a>
                                </li>
                            </ul>
                        </div>
                    </div>

                    <div class="row no-gutters" style="margin: 1rem 0px 1rem 0px">
                        <div class="tab-content row no-gutters col-12" id="pills-tabContent">
                            <div class="tab-pane fade show active col-12" id="pills-all" role="tabpanel"
                                aria-labelledby="pills-all-tab">
                                <div v-if="!task_list.length" class="offset-1 col-10">暂无任务</div>
                                <div v-for="(each, index) in task_list" class="task_card offset-1 col-10" :key="index"
                                    style="height: 6rem; margin-bottom: 1.5rem; cursor: pointer; padding-right: 0px" v-on:click.prevent="mySwitch(['taskDetail', each.id])">
                                    <div style="display: inline-block;">
                                        <div class="lead" style="color: #333333; margin-top: 1.2rem">{{each.task_name}}</div>
                                        <small style="color: #999999;">参与成员:
                                            <span v-for="each_member in each.members">&nbsp;{{each_member}}</span>
                                            ...</small>
                                    </div>
                                    <div v-if="each.is_creator" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                        <a @click.stop="delete_task(each.id, index)" class="btn btn-danger" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff">删除</a>
                                    </div>
                                    <div v-if="each.is_leader && (!each.is_creator)" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                    </div>
                                </div>
                            </div>
                            <div class="tab-pane fade row no-gutters col-12" id="pills-ongoing" role="tabpanel"
                                aria-labelledby="pills-ongoing-tab">
                                <div v-for="(each, index) in task_list" class="task_card offset-1 col-10" v-if="each.task_status == 1"
                                    style="height: 6rem; margin-bottom: 1.5rem; padding: 1.2rem; cursor: pointer;" :key="index" v-on:click.prevent="mySwitch(['taskDetail', each.id])">
                                    <div style="display: inline-block;">
                                        <div class="lead" style="color: #333333">{{each.task_name}}</div>
                                        <small style="color: #999999">参与成员:
                                            <span v-for="each_member in each.members">&nbsp;{{each_member}}</span>
                                            ...</small>
                                    </div>
                                    <div v-if="each.is_creator" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                        <a @click.stop="delete_task(each.id, index)" class="btn btn-danger" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff">删除</a>
                                    </div>
                                    <div v-if="each.is_leader && (!each.is_creator)" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                    </div>
                                </div>
                            </div>
                            <div class="tab-pane fade row no-gutters col-12" id="pills-pause" role="tabpanel"
                                aria-labelledby="pills-pause-tab">
                                <div v-for="(each, index) in task_list" class="task_card offset-1 col-10" v-if="each.task_status == 0"
                                    style="height: 6rem; margin-bottom: 1.5rem; padding: 1.2rem; cursor: pointer;" :key="index" v-on:click.prevent="mySwitch(['taskDetail', each.id])">
                                    <div style="display: inline-block;">
                                        <div class="lead" style="color: #333333">{{each.task_name}}</div>
                                        <small style="color: #999999">参与成员:
                                            <span v-for="each_member in each.members">&nbsp;{{each_member}}</span>
                                            ...</small>
                                    </div>
                                    <div v-if="each.is_creator" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                        <a @click.stop="delete_task(each.id, index)" class="btn btn-danger" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff">删除</a>
                                    </div>
                                    <div v-if="each.is_leader && (!each.is_creator)" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                    </div>
                                </div>
                            </div>
                            <div class="tab-pane fade row no-gutters col-12" id="pills-finished" role="tabpanel"
                                aria-labelledby="pills-finished-tab">
                                <div v-for="(each, index) in task_list" class="task_card offset-1 col-10" v-if="each.task_status == -1"
                                    style="height: 6rem; margin-bottom: 1.5rem; padding: 1.2rem; cursor: pointer;" :key="index" v-on:click.prevent="mySwitch(['taskDetail', each.id])">
                                    <div style="display: inline-block;">
                                        <div class="lead" style="color: #333333">{{each.task_name}}</div>
                                        <small style="color: #999999">参与成员:
                                            <span v-for="each_member in each.members">&nbsp;{{each_member}}</span>
                                            ...</small>
                                    </div>
                                    <div v-if="each.is_creator" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                        <a @click.stop="delete_task(each.id, index)" class="btn btn-danger" style="border-radius: 0px; height: 33.3%; width: 100%; line-height: 100%; color: #ffffff">删除</a>
                                    </div>
                                    <div v-if="each.is_leader && (!each.is_creator)" style="float: right; width: 6rem; height: 100%; display: none" class="options">
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click="mySwitch(['taskDetail', each.id])">查看详情</a>
                                        <a class="btn btn-primary" style="border-radius: 0px; height: 50%; width: 100%; line-height: 200%; color: #ffffff" v-on:click.stop="mySwitch(['editTask', each.id])">编辑</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div v-if="show_more" class="row no-gutters"
                        style="background-color: #E4EBFF; height: 4rem; cursor: pointer; display: none">
                        <div style="text-decoration: none; color: #999999; margin-left: auto; margin-right: auto">
                            <div style="position: relative; transform: translateY(-50%); top: 50%">
                                <div>点击加载更多</div>
                                <div style="text-align: center; transform: rotate(180deg)">^</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .add::before {
        content: '';
        position: absolute;
        left: 50%;
        top: 53%;
        width: 30px;
        margin-left: -18px;
        margin-top: -4px;
        border-top: 5px solid #C9C9C9;
    }

    .add::after {
        content: '';
        position: absolute;
        left: 50%;
        top: 53%;
        height: 30px;
        margin-left: -5px;
        margin-top: -17px;
        border-left: 5px solid #C9C9C9;
    }

    .table td,
    .table th {
        border-top: 0px;
        vertical-align: middle;
    }

    .card {
        margin: 1.2rem 0rem;
        border-radius: 0px;
        border-bottom: 0px;
        border-left: 0px;
        border-right: 0px;
        border-top: 0.25rem solid #448AFF;
    }

    .task_card:hover {
        box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16);
        background-image: url("/static/pic/each_task_card_bgi.png");
        background-repeat: no-repeat;
    }

    .task_card:hover>.options{
        display: block !important;
    }

    .task_card:hover>div {
        margin-left: 55px;
    }

    .avatar:hover>a>div {
        display: block !important;
        position: absolute;
    }

    .add:hover {
        background-image: url("/static/pic/create_task_button_bgi.png");
        background-repeat: round;
        background-color: #448AFF;
    }

    .add:hover::after,
    .add:hover::before {
        left: 25%;
    }

    .add:hover>a {
        display: block !important;
        font-size: 1.2rem;
        left: 10%;
        position: relative;
    }

    .nav-link {
        padding: 0rem 1rem;
        border: 0rem;
    }

    .nav-tabs {
        border: 0rem;
    }

    .nav-link.active {
        border: 0px;
        border-bottom: 1px solid #2962FF;
    }
</style>

<script>
    export default {
        data() {
            return {
                task_list: [],
                show_more: true,
            }
        },
        created() {
            this.getInfo();
        },
        props: ['globle_props'],
        methods: {
            getInfo: function(){
                this.$http.get("/account/task_list/").then(result => {
                    if (result.status == 200){
                        this.task_list = result.body.info.content;
                    }
                })
            },
            delete_task: function (task_id, index){
                this.$http.get('/task/delete_task/' + task_id + '/');
                this.task_list.splice(index, 1);
            },
            mySwitch: function(target) {
                this.$emit('switch', target);
            }
        }
    }
</script>
