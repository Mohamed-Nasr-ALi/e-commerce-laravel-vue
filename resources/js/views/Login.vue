<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">Login</div>

                    <div class="card-body">
                        <form>
                            <div class="form-group row">
                                <label for="email" class="col-sm-4 col-form-label text-md-right">E-Mail Address</label>

                                <div class="col-md-6">
                                    <input id="email" maxlength="100" type="email" class="form-control" v-model="email" required autofocus>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label for="password" class="col-md-4 col-form-label text-md-right">Password</label>

                                <div class="col-md-6">
                                    <input id="password" type="password" class="form-control" v-model="password" required>
                                </div>
                            </div>

                            <div class="form-group row mb-0">
                                <div class="col-md-8 offset-md-4">
                                    <button type="submit" class="btn btn-primary" @click="handleSubmit">
                                        Login
                                    </button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                email : "",
                password : ""
            }
        },
        methods : {
            handleSubmit(e){
                e.preventDefault()

                if (this.password.length > 0) {
                  axios.post('api/login', {
                    email: this.email,
                    password: this.password
                  })
                  .then(response => {
                    let is_admin = response.data.user.is_admin
                    sessionStorage.setItem('user',JSON.stringify(response.data.user))
                    sessionStorage.setItem('jwt',response.data.token)

                    if (sessionStorage.getItem('jwt') != null){
                        this.$emit('loggedIn')
                        if(this.$route.params.nextUrl != null){
                            this.$router.push(this.$route.params.nextUrl)
                        }
                        else {
                            if(is_admin== 1){
                                this.$router.push('admin').catch((e)=>{
                                    console.log(e)})
                            }
                            else {
                                this.$router.push('dashboard').catch((e)=>{
                                    console.log(e)})
                            }
                        }
                    }
                  })
                  .catch(function (error) {
                      Swal.fire({
                          icon: 'error',
                          title: 'Oops...',
                          text: 'Something went wrong!'
                      })
                  });
                }
            }
        },
    }
</script>

