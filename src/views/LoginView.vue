<template>
    <div class="container">
        <div class="row  vh-100 mt-3 justify-content-center align-items-center">
            <div class="col-4">
                <h1>Login Form</h1>
                <hr>
                <form action="" @submit.prevent="login" ref="loginForm">
                    <div class="form-group mb-3">
                        <label for="">Email address</label>
                        <input type="email" class="form-control" name="email" placeholder="Enter email">
                    </div>
                    <div class="form-group mb-3">
                        <label for="">Password</label>
                        <input type="password" class="form-control" name="password" placeholder="Password">
                    </div>
                    <button type="submit" class="btn btn-primary">Login</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import { mapGetters, mapState } from 'vuex'
    export default {
        computed: {
            ...mapState([
                'auth',
                'token'
            ]),
            ...mapGetters([
                'getUrl'
            ])
        },
        methods: {
            login() {
                let formData = new FormData(this.$refs.loginForm);
                fetch(this.getUrl('/login'),{
                    method:"POST",
                    body:formData
                }).then(res => res.json())
                .then(json => {
                    if(json.success == true){
                        this.$store.dispatch('setAuth',json.auth);
                        this.$store.dispatch('setToken',json.token);
                        this.$router.push('/dashboard')
                    }
                })
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>