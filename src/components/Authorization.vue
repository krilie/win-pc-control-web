<template>
    <v-container>
            <v-form>
                <v-layout>
                    <v-flex xs12 md4 class="align-content-center">
                        <v-text-field
                                v-model="secretText"
                                :counter="64"
                                label="secret text"
                                required
                        ></v-text-field>
                        <v-btn @click="Vertify" class="align-center">
                            提交登录
                        </v-btn>
                    </v-flex>
                </v-layout>
            </v-form>
    </v-container>
</template>

<script>
    export default {
        name: "Authorization",
        data: () => ({
            secretText: ""
        }),
        methods:{
            Vertify(){
                this.$axios.post("/user/vertify",{secret:this.secretText}).then((response)=>{
                    this.$emit('vertifyok',true,null) //发送事件，通知父组件
                }).catch((error)=>{
                    this.$emit('vertifyok',false,error.response.status+": "+ error.response.statusText)
                })
            }
        }
    }
</script>

<style scoped>

</style>
