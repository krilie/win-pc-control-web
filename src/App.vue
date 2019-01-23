<template>
    <v-app>
        <v-toolbar>
            <v-spacer></v-spacer>
            <v-toolbar-title v-text="title" class="text-md-center"></v-toolbar-title>
            <v-spacer></v-spacer>
            <!--<v-btn icon>-->
            <!--<v-icon>more_vert</v-icon>-->
            <!--</v-btn>-->
        </v-toolbar>

        <v-content>
            <!--<HelloWorld/>-->
            <Authorization v-show="isAuthorizationed" @vertifyok="VertifyOk"></Authorization>
            <WinPcControl v-show="!isAuthorizationed"></WinPcControl>
        </v-content>

        <v-snackbar
                v-model="snackbar"
                :bottom=true
                :right=true
                :timeout="snackbarTimeOut"
                :multi-line="true"
        >
            {{ snackbarText }}
            <v-btn
                    color="pink"
                    flat
                    @click="snackbar = false"
            >
                Close
            </v-btn>
        </v-snackbar>

    </v-app>
</template>

<script>
    // import HelloWorld from './components/HelloWorld'
    import Authorization from './components/Authorization'
    import WinPcControl from './components/WinPcControl'

    export default {
        name: 'App',
        components: {
            // HelloWorld
            Authorization, WinPcControl
        },
        data() {
            return {
                title: "pc control app",
                isAuthorizationed: false,
                snackbar: false,
                snackbarText: "dfas",
                snackbarTimeOut: 60000
            }
        },
        methods: {
            Vertify() {
                this.$axios.post("/auth/verity").then((response) => {
                    this.isAuthorizationed = true
                }).catch((error) => {
                    this.snackbar = true;
                    this.snackbarText = error.response.status + ":" + error.response.statusText
                })
            },
            VertifyOk(ok, msg) {
                if (ok) {
                    this.isAuthorizationed = true
                } else {
                    this.snackbar = true;
                    this.snackbarText = msg
                }
            }
        },
        created() {
            this.Vertify()
        }
    }
</script>
