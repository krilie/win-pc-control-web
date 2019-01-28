<template>
    <v-container justify-center align-center>
        <v-spacer></v-spacer>

        <v-layout column>
            <v-text-field
                    v-model="secretText"
                    :counter="64"
                    label="secret text"
                    required
            ></v-text-field>

            <v-container>
                <v-spacer></v-spacer>
                <v-btn @click="Vertify">
                    提交登录
                </v-btn>
                <v-spacer></v-spacer>
            </v-container>

        </v-layout>

        <v-spacer></v-spacer>

    </v-container>
</template>

<script>
    export default {
        name: "Authorization",
        data: () => ({
            secretText: ""
        }),
        methods: {
            Vertify() {
                let data = new FormData();
                data.append("secret_key", this.secretText)
                this.$axios.post("/auth/login", data).then((response) => {
                    this.$emit('vertifyok', true, null) //发送事件，通知父组件
                }).catch((error) => {
                    this.$emit('vertifyok', false, error.response.status + ": " + error.response.statusText)
                })
            }
        }
    }
</script>

<style scoped>

</style>
