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
                    <h1 class="h4 text-gray-900 mb-4">Add Category</h1>
                  </div>
                  <form @submit.prevent="categoryInsert" enctype="multipart/form-data">
                    <div class="form-group">
                        <div class="form-row">
                            <div class="col-md-12">
                                  <input type="text" v-model="form.category_name" placeholder="Enter The category name" class="form-control" >
                                    <!-- <label class="custom-file-label" for="customFile"></label> -->
                                <small class="text-danger" v-if="errors.category_name"> {{errors.category_name[0]}} </small>
                            </div>

                        </div>
                    </div>
                    <div class="form-group">
                        <button type="submit" class="btn btn-primary btn-block">Submit</button>
                    </div>
                    <hr>
                  </form>
                  <hr>
                  <!-- <div class="text-center">
                    <router-link class="font-weight-bold small" to="/">Already have an account?</router-link>
                  </div> -->
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
        if(!User.loggedIn()) {
            this.$router.push({name: '/'})
        }
    },
data() {
      return {
          form:{
            category_name: null,
          },
          errors: {}
      }
},
    methods: {
        categoryInsert() {
            axios.post('/api/category', this.form)
            .then( ()=> {
                this.$router.push({name: 'category'})
                Notification.success()
            })
            .catch(error => this.errors = error.response.data.errors)
        },
    }
    }
</script>
