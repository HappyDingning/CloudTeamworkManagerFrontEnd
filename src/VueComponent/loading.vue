<template>
    <div>
        <div v-if="show_alert" style="z-index: 999; position: fixed; width: 100%;"
            class="alert" :class="[is_success? 'alert-success' : 'alert-warning']"
            role="alert">
            {{ alert_text }}
            <button class="close" @click="show_alert = !show_alert">
                <span aria-hidden="true">&times;</span>
            </button>
        </div>
        <div style="display: block; z-index: 998; position: fixed; background-color: rgba(255, 255, 255, 0.7); width: 100%; height: 100%; text-align: center"
            :style="{display: loading_display_mode}">
            <h1 style="position: relative; top: 20%">
                正在提交，请稍后...
            </h1>
            <img src="/static/pic/betta_fish.png" style="height: 100%;">
        </div>
    </div>
</template>

<style scoped>
    .alert{
        border-radius: 0px;
    }
</style>

<script>
    export default {
        data() {
            return {
                alert_text: "操作成功",
                is_success: true,
                show_alert: true,
                loading_display_mode: 'block',
            }
        }
    }
</script>
