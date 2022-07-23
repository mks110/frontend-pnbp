<template>

<body class="hold-transition login-page">
    <div class="container-fluid mt-5">
        <div class="row justify-content-center">
            <!-- <div class="card-header text-center">
          <img :src="'/img/logo.png'" alt="" style="width:80px">
        </div> -->
            <div class="col-md-4">
                <div v-if="loginFailed" class="alert alert-danger">
                        user_name atau Password Anda salah.
                    </div>

                <div class="login-box">
                    <div class="card card-outline card-warning">
                        <div class="card-header text-center">
                            <img :src="'/img/logo.png'" alt="" style="width:80px">
                        </div>
      
                        <div class="card-body">
                            <p class="login-box-msg center">Silahkan Login ke Aplikasi</p>
                            <hr>
                            <form @submit.prevent="login">
                                <div class="form-group">
                                    <label>Username</label>
                                    <input type="user_name" v-model="user.user_name" class="form-control"
                                        placeholder="Username">
                                </div>
                                <div v-if="validation.user_name" class="mt-2 alert alert-danger">
                                    {{ validation.user_name[0] }}
                                </div>

                                <div class="form-group">
                                    <label>Password</label>
                                    <input type="password" v-model="user.password" class="form-control"
                                        placeholder="Password">
                                </div>
                                <div v-if="validation.password" class="mt-2 alert alert-danger">
                                    {{ validation.password[0] }}
                                </div>
                                <button type="submit" class="btn btn-primary btn-block">LOGIN</button>
                            </form>
                        <p class="mb-0">
                            <router-link to="/LupaPasword">lupa Password</router-link>
                        </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</body>

</template>

<script>
    import { reactive, ref } from 'vue'
    import { useRouter } from 'vue-router'
    import axios from 'axios'

    export default {

        setup() {

            //inisialisasi vue router on Composition API
            const router = useRouter()

            //state user
            const user = reactive({
                user_name: '',
                password: '',
            })

            //state validation
            const validation = ref([])

            //state loginFailed
            const loginFailed = ref(null)

            //method login
            function login() {

                //define variable 
                let user_name = user.user_name
                let password = user.password

                //send server with axios
                axios.post('http://localhost:8000/api/login', {
                        user_name,
                        password,
                })
                .then(response => {

                    

                        //set token
                        localStorage.setItem('token', response.data.token)

                        //redirect ke halaman dashboard
                        return router.push({
                            name: 'dashboard'
                        })
                    

                    //set state loggedIn to true
                    


                }).catch(error => {
                    //set validation dari error response
                    validation.value = error.response.data
                })

            }

            return {
                user,           // <-- state user
                validation,     // <-- state validation 
                loginFailed,    // <-- state loggedIn
                login           // <-- method login
            }

        }

    }
</script>