<template>
    <div class="container">
        <div class="row  vh-100 mt-3 justify-content-center align-items-center">
            <div class="col-4">
                <h1>Register Form</h1>
                <hr>
                <form action="" @submit.prevent="register" ref="registerForm">
                    <div class="form-group mb-3">
                        <label for="">Name</label>
                        <input type="text" class="form-control" name="name" placeholder="Your Name">
                    </div>
                    <div class="form-group mb-3">
                        <label for="">Email address</label>
                        <input type="email" class="form-control" name="email" placeholder="Enter email">
                    </div>
                    <div class="form-group mb-3">
                        <label for="">Password</label>
                        <input type="password" class="form-control" name="password" placeholder="Password">
                    </div>
                    <div class="form-group mb-3">
                        <label for="">Confirm Password</label>
                        <input type="password" class="form-control" name="password_confirmation" placeholder="Confirm Password">
                    </div>
                    <button type="submit" class="btn btn-primary">Register</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import { mapGetters } from 'vuex';
    export default {
        computed: {
           ...mapGetters([
            'getUrl'
           ])
        },
        methods: {
            register() {
                let formData = new FormData(this.$refs.registerForm);
                fetch(this.getUrl("/register"),{
                    method:"POST",
                    body   : formData
                }).then(res => res.json())
                .then(json => {
                    if(json.success == true){
                        this.$router.push('/login');
                    }
                })
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>