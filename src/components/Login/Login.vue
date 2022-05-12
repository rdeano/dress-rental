<template> 
    <div class="login-box">
    <div class="login-logo">
        <a href="javascript:void(0)"><b>Dress Rental System </b></a>
    </div>
    <!-- /.login-logo -->
    <div class="card">
        <div class="card-body login-card-body">
        <p class="login-box-msg">Sign in to start your session</p>

        <form @submit="signin()">
             <div class="alert alert-danger" v-if="error">
                Invalid Username or Password
            </div>

            <div class="input-group mb-3">
            <input type="email" v-model="email" class="form-control" placeholder="email">
            <div class="input-group-append">
                <div class="input-group-text">
                <span class="fas fa-user"></span>
                </div>
            </div>
            </div>
            <div class="input-group mb-3">
            <input type="password" v-model="password" class="form-control" placeholder="Password">
            <div class="input-group-append">
                <div class="input-group-text">
                <span class="fas fa-lock"></span>
                </div>
            </div>
            </div>
            <div class="row">
            <div class="col-8">
                <div class="icheck-primary">
                <input type="checkbox" id="remember">
                <label for="remember">
                    Remember Me
                </label>
                </div>
            </div>
            <!-- /.col -->
            <div class="col-4">
                <button type="submit" class="btn btn-primary btn-block" @click="signin()">Sign In</button>
            </div>
            <!-- /.col -->
            </div>
        </form>

        <div class="social-auth-links text-center mb-3">
            <!-- <p>- OR -</p>
            <a href="#" class="btn btn-block btn-primary">
            <i class="fab fa-facebook mr-2"></i> Sign in using Facebook
            </a>
            <a href="#" class="btn btn-block btn-danger">
            <i class="fab fa-google-plus mr-2"></i> Sign in using Google+
            </a> -->
        </div>
        <!-- /.social-auth-links -->

        <!-- <p class="mb-1">
            <a href="forgot-password.html">I forgot my password</a>
        </p>
        <p class="mb-0">
            <a href="register.html" class="text-center">Register a new membership</a>
        </p> -->
        </div>
        <!-- /.login-card-body -->
    </div>
    </div>
    <!-- /.login-box -->
</template>

<script lang="ts">

import axios from 'axios'
import sweetalert2 from 'sweetalert2'
import { defineComponent, ref, watch } from 'vue';

export default defineComponent({
    name : 'Login',
    components: {},
    setup() {
        const email = ref('');
        const password = ref('')
        const error = ref(false);

        function signin() { 
            event?.preventDefault();
            let data = {
                email : email.value,
                password : password.value
            }

            axios.post('login',data)
                .then(
                    res => {
                        document.body.classList.remove('hold-transition')  
                        document.body.classList.remove('login-page')

                        //@ts-ignore
                        this.$store.commit("setAuthentication",true)
                        //@ts-ignore
                        this.$router.push('home')
                    }
                ).catch(
                    err => {
                        console.log(err);
                        error.value = true;
                    }
                )           
        }

        watch(email,(newVal) => {
            error.value = false;
        })

        watch(password,(newVal) => {
            error.value = false;
        })

        return {
            email,
            password,
            signin,
            error
        }
    }
   
})
</script>