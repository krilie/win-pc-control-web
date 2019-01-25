<template>
    <v-container fluid>
        <v-layout row wrap>
            <v-flex xs12>
                <v-switch v-model="monitor">
                    <template slot="label">
                        打开屏幕
                    </template>
                </v-switch>
            </v-flex>
            <v-flex xs12 sm6>

                <v-slider
                        v-model="volume"
                        :min="0"
                        :max="100"
                ></v-slider>
            </v-flex>

            <v-flex xs12>
                <v-layout row wrap>
                    <v-btn class="primary" @click="VolumeUpDown('+')">音量+</v-btn>
                    <v-btn class="primary" @click="VolumeUpDown('-')">音量-</v-btn>
                </v-layout>

            </v-flex>

            <v-flex xs12>
                <v-btn-toggle v-model="toggle_exclusive">
                    <v-btn flat large @click="MediaControl('prev')">
                        <v-icon :src="required('../assets/prev.png')"></v-icon>
                    </v-btn>
                    <v-btn flat large @click="MediaControl('stop')">
                        <v-icon>stop</v-icon>
                    </v-btn>
                    <v-btn flat large @click="MediaControl('play')">
                        <v-icon>play_arrow</v-icon>
                    </v-btn>
                    <v-btn flat large @click="MediaControl('pause')">
                        <v-icon>pause</v-icon>
                    </v-btn>
                    <v-btn flat large @click="MediaControl('mute')">
                        <v-icon>volume_mute</v-icon>
                    </v-btn>
                    <v-btn flat large @click="MediaControl('next')">
                        <v-icon>skip_next</v-icon>
                    </v-btn>
                </v-btn-toggle>
            </v-flex>


            <v-flex xs12>
                <v-btn class="warning" @click="SysControlShutDown">关机</v-btn>
            </v-flex>
        </v-layout>

    </v-container>
</template>

<script>
    export default {
        name: "WinPcControl",
        data: () => {
            return {
                monitor: "true", //"off"
                volume: 50
            }
        },
        watch: {
            //true on false off
            monitor(val) {
                let action = "monitor_on"
                if (val === true) {
                    action = "monitor_on"
                } else {
                    action = "monitor_off"
                }
                //发出请求
                let data = new FormData();
                data.append("action", action)
                this.$axios.post("/sysctl/action", data).then((response) => {
                    //调用成功
                    this.$emit("ShowSnackarbar", "成功:" + action, 1000);
                }).catch((error) => {
                    this.monitor = false; //可以这样写
                    this.$emit("ShowSnackarbar", error.response.status + ":" + error.response.statusText, 1000);
                })
            },
            volume(val,oldval) {
                //音量调整
                let data = new FormData(); //form 数据 post
                data.append('value', val);
                this.$axios.post("/volume/value", data).then((response) => {
                    //this.$emit("ShowSnackarbar", "音量:" + val, 1000);
                    //成功不做什么
                }).catch((error) => {
                    this.volume=oldval;//可以这样写
                    this.$emit("ShowSnackarbar", error.response.status + ":" + error.response.statusText, 1000);
                })
            }
        },
        methods: {
            //屏幕开关 关机
            // monitor_on
            // monitor_off
            // shutdown
            SysControlShutDown() {
                //发出请求
                let data = new FormData();
                data.append("action", "shutdown")
                this.$axios.post("/sysctl/action", data).then((response) => {
                    //调用成功
                }).catch((error) => {
                    this.$emit("ShowSnackarbar", error.response.status + ":" + error.response.statusText, 1000);
                })
            },
            //媒体控制
            // play pause stop next prev
            // vol_up vol_down mute
            MediaControl(action) {
                let data = new FormData();
                data.append("action", action)
                this.$axios.post("/media/status", data).then((response) => {
                    this.$emit("ShowSnackarbar", "成功：" + response.body + " " + action, 1000);
                }).catch((error) => {
                    this.$emit("ShowSnackarbar", error.response.status + ":" + error.response.statusText, 1000);
                })
            },
            VolumeUpDown(action) {
                if (action === "+") {
                    this.MediaControl("vol_up");
                } else if (action === "-") {
                    this.MediaControl("vol_down");
                } else {
                    //没有时间参数的调用
                    this.$emit("ShowSnackarbar", "媒体控制参数错误");
                }
            }
        }
    }
</script>

<style scoped>

</style>
