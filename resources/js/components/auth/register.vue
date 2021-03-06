<template>
   <div >
        <div class="row justify-content-center">
      <div class="col-xl-10 col-lg-12 col-md-9">
        <div class="card shadow-sm my-5">
          <div class="card-body p-0">
            <div class="row">
              <div class="col-lg-12">
                <div class="login-form">
                  <div class="text-center">
                    <h1 class="h4 text-gray-900 mb-4">Register</h1>
                  </div>
                  <form @submit.prevent="signup">
                    <div class="form-group" >
                      <input type="text" v-model="form.name" class="form-control" id="exampleInputFirstName" placeholder="Enter Your full Name">
                      <small class="text-danger" v-if="errors.name"> {{errors.name[0]}} </small>
                    </div>
                    <div class="form-group">
                      <input type="email" v-model="form.email" class="form-control" id="exampleInputEmail" aria-describedby="emailHelp"
                        placeholder="Enter Email Address">
                      <small class="text-danger" v-if="errors.email"> {{errors.password[0]}} </small>
                    </div>
                    <div class="form-group">
                      <input type="password" v-model="form.password" class="form-control" id="exampleInputPassword" placeholder="Password">
                      <small class="text-danger" v-if="errors.password"> {{errors.password[0]}} </small>
                    </div>
                    <div class="form-group">
                      <input type="password" v-model="form.password_confirmation" class="form-control" id="exampleInputPasswordRepeat"
                        placeholder="Confirm Password">
                      <small class="text-danger" v-if="errors.password_confirmation"> {{errors.password_confirmation[0]}} </small>
                    </div>
                    <div class="form-group">
                      <button type="submit" class="btn btn-primary btn-block">Register</button>
                    </div>
                    <hr>
                  </form>
                  <hr>
                  <div class="text-center">
                    <router-link class="font-weight-bold small" to="/">Already have an account?</router-link>
                  </div>
                  <div class="text-center">
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
   </div>
</template>

<script>
export default {
    created() {
        if(User.loggedIn()) {
            this.$router.push({name: 'home'})
        }
    },
    data() {
        return {
            form: {
                email: null,
                password: null,
                password_confirmation: null,
                name: null
            },
        errors: {}
        }
    },
 methods: {
        signup() {
            axios.post('/api/auth/signup', this.form)
            .then(res => {
                User.responseAfterLogin(res)
                this.$router.push({name: 'home'})
                Toast.fire({
                    icon: 'success',
                    title: 'Account Create Successfully!'
                })
            })
            .catch(error => this.errors = error.response.data.errors)
            .catch(
                Toast.fire({
                    icon: 'warning',
                    title: 'Invalid Email or Password '
                })
            )
        }
    }
}
</script>
<style type="text/css">

</style>
