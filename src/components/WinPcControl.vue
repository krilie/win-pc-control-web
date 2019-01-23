<template>
    <v-container fluid>
        <v-layout row wrap >
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
                        prepend-icon="volume_up"
                ></v-slider>
            </v-flex>

            <v-flex xs12>
                <v-layout row wrap>
                    <v-btn class="primary">音量+</v-btn>
                    <v-btn class="primary">音量-</v-btn>
                </v-layout>

            </v-flex>

            <v-flex xs12>
                <v-btn-toggle  v-model="toggle_exclusive">
                    <v-btn flat large>
                        <v-icon>skip_previous</v-icon>
                    </v-btn>
                    <v-btn flat large>
                        <v-icon>stop</v-icon>
                    </v-btn>
                    <v-btn flat large>
                        <v-icon>play_arrow</v-icon>
                    </v-btn>
                    <v-btn flat large>
                        <v-icon>pause</v-icon>
                    </v-btn>
                    <v-btn flat large>
                        <v-icon>volume_mute</v-icon>
                    </v-btn>
                    <v-btn flat large>
                        <v-icon>skip_next</v-icon>
                    </v-btn>
                </v-btn-toggle>
            </v-flex>



            <v-flex xs12>
                <v-btn class="warning">关机</v-btn>
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

            },
            volume(val) {

            }
        },
        methods: {
            //屏幕开关 关机
            // monitor_on
            // monitor_off
            // shutdown
            SysControl() {

            },
            //音量值 0-100
            Volume(volue) {

            },
            //媒体控制
            // play pause stop next prev
            // vol_up vol_down mute
            MediaControl(action) {

            }
        }
    }
</script>

<style scoped>

</style>
