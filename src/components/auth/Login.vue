<template>
    <div class="push-down">
        <div class="container">
            <div class="row">
                <div class="col-md-4 col-md-offset-4">
                    <div class="alert alert-danger alert-dismissable" v-if="error">
                        <a href="#" class="close" @click="error = ''">&times;</a>
                        <strong>{{error}}</strong>
                    </div>
                    <div class="push-down"></div>
                    <div class="panel panel-primary">
                        <div class="panel-heading">
                            login | register
                        </div>
                        <div class="panel-body">
                            <form>
                                <div class="form-group">
                                    <input type="email" class="form-control" placeholder="Email" v-model="email">
                                </div>
                                <div class="form-group">
                                    <input type="password" class="form-control" placeholder="Password" v-model="password" v-on:keyup.enter="login()">
                                </div>
                            </form>
                        </div>
                        <div class="panel-footer">
                            <div class="row login-btn-row">
                                <button class="btn btn-default pull-left" @click="login()" :disabled="loginState">Login <i class="fa fa-sign-in" aria-hidden="true"></i></button>
                                <button class="btn btn-success pull-right" @click="register" :disabled="loginState">Register <i class="fa fa-user-plus"></i></button>
                            </div>
                        </div>
                    </div>
                    <div class="push-down" align="center">
                        <button class='btn btn-danger btn-xs' @click="signInGoogle()">Google <i class="fa fa-google"></i></button>
                        <button class="btn facebook-color btn-xs" @click="signInFacebook()">Facebook <i class="fa fa-facebook"></i></button>
                        <button class="btn btn-primary btn-xs" @click="signInTwitter()">Twitter <i class="fa fa-twitter"></i></button>
                        <button class="btn github-color btn-xs" @click="signInGithub()">Github <i class="fa fa-github"></i></button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        mounted() {
        /**
         * the recommended way to handle redirections after login/register is to do it manually through methods.
         * 
         * @see routesActions
         */               
        },
        firebase() {
            return {
                activeUsers: this.$store.state.database.child('activeUsers')
            }
        },
        data() {
            return {
                email: '',
                password: '',
                loginState: false,
                error: '',
            }
        },
        computed: {},
        methods: {
            login() {
                this.loginState = true
                this.$auth.loginWithEmailAndPassowrd({
                    email: this.email,
                    password: this.password,
                    result: (user) => {
                        let uid = user.uid
                        this.$firebaseRefs.activeUsers.update({
                            [uid]: {
                                userName: user.displayName,
                                userEmail: user.email
                            }
                        })
                    this.routesAction('/app');
                    },
                    error: (error) => {
                        this.error = error.message
                    }
                })
                this.middleware()
            },
            register() {
                this.loginState = true;

                this.$auth.registerWithEmailAndPassword({
                    email: this.email,
                    password: this.password,
                    result: (user) => {
                        let uid = user.uid
                        this.$firebaseRefs.activeUsers.update({
                            [uid]: {
                                userName: user.displayName,
                                userEmail: user.email
                            }
                        })
                    this.routesAction('/update-profile');
                    },
                    error: (error) => {
                        this.error = error.message
                    }
                });
            },
            middleware() {
                this.password = ''
                this.loginState = false
            },
            signInGoogle() {
                this.$auth.signInWithGoogle({
                    result: (result) => {
                        // This gives you a Google Access Token. You can use it to access the Google API.
                        // console.log("Token : " + result.credential.accessToken)
                        // The signed-in user info.
                        let uid = result.user.uid
                        this.$firebaseRefs.activeUsers.update({
                            [uid]: {
                                userName: result.user.displayName,
                                userEmail: result.user.email
                            }
                        })
                    this.routesAction('/app');
                    },
                    error: (error) => {
                        this.error = error.message
                    }
                })
            },
            signInFacebook() {
                this.$auth.signInWithFacebook({
                    result: (result) => {
                        //console.log("Token : " + result.credential.accessToken)
                        let uid = result.user.uid
                        this.$firebaseRefs.activeUsers.update({
                            [uid]: {
                                userName: result.user.displayName,
                                userEmail: result.user.email
                            }
                        })
                    this.routesAction('/app');
                    },
                    error: (error) => {
                        this.error = error.message
                    }
                })
            },
            signInTwitter() {
                this.$auth.signInWithTwitter({
                    result: (result) => {
                        //console.log("Token : " + result.credential.accessToken)
                        let uid = result.user.uid
                        this.$firebaseRefs.activeUsers.update({
                            [uid]: {
                                userName: result.user.displayName,
                                userEmail: result.user.email
                            }
                        })
                    this.routesAction('/app');
                    },
                    error: (error) => {
                        this.error = error.message
                    }
                })
            },
            signInGithub() {
                this.$auth.signInWithGithub({
                    result: (result) => {
                        //console.log("Token : " + result.credential.accessToken)
                        let uid = result.user.uid
                        this.$firebaseRefs.activeUsers.update({
                            [uid]: {
                                userName: result.user.displayName,
                                userEmail: result.user.email
                            }
                        })
                    this.routesAction('/app');
                    },
                    error: (error) => {
                        this.error = error.message
                    }
                })
            },
            routesAction(action){
                this.$destroy()
                this.$router.push(action)
                this.$router.go(1)
            }
        }
    }
</script>