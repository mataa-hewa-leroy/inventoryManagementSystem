<template>
   <div>
    <div class="row justify-content-center">
      <div class="col-xl-10 col-lg-12 col-md-9">
        <div class="card shadow-sm my-5">
          <div class="card-body p-0">
            <div class="row">
              <div class="col-lg-12">
                <div class="login-form">
                  <div class="text-center">
                    <h1 class="h4 text-gray-900 mb-4">Supplier Update</h1>
                  </div>
                <form @submit.prevent="supplierUpdate" enctype="multipart/form-data">
                    <div class="form-group" >
                        <div class="form-row">
                            <div class="col-md-6">
                                <input type="text" v-model="form.name" class="form-control" id="exampleInputFirstName" placeholder="Enter Your Full Name">
                                <small class="text-danger" v-if="errors.name"> {{errors.name[0]}} </small>
                            </div>
                            <div class="col-md-6">
                                <input type="email" v-model="form.email" class="form-control" id="exampleInputEmail" aria-describedby="emailHelp"
                                placeholder="Enter The Email">
                            <small class="text-danger" v-if="errors.email"> {{errors.email[0]}} </small>
                            </div>
                            </div>
                    </div>
                     <div class="form-group" >
                        <div class="form-row">
                            <div class="col-md-6">
                                <input type="text" v-model="form.adress" class="form-control" id="exampleInputFirstName" placeholder="Enter Adress">
                                <small class="text-danger" v-if="errors.adress"> {{errors.adress[0]}} </small>
                            </div>
                            <div class="col-md-6">
                                <input type="text" v-model="form.phone" class="form-control" id="exampleInputEmail" aria-describedby="emailHelp"
                                placeholder="Enter Your Phone Number">
                            <small class="text-danger" v-if="errors.phone"> {{errors.phone[0]}} </small>
                            </div>
                            </div>
                    </div>
                     <div class="form-group" >
                        <div class="form-row">
                            <div class="col-md-6">
                                <input type="text" v-model="form.shopname" class="form-control" id="exampleInputFirstName" placeholder="Enter The Shopname">
                                <small class="text-danger" v-if="errors.shopname"> {{errors.shopname[0]}} </small>
                            </div>
                            </div>
                    </div>
                    <div class="form-group">
                        <div class="form-row">
                            <div class="col-md-6">
                                  <input type="file" @change="onFileSelected" class="custom-file-input" id="customFile">
                                    <label class="custom-file-label" for="customFile">Choose file</label>
                                <small class="text-danger" v-if="errors.new_photo"> {{errors.photo[0]}} </small>
                            </div>
                            <div class="col-md-6">
                                 <img :src="form.photo" style="height: 40px; width: 40px;" alt="">
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
        let id = this.$route.params.id
        axios.get('/api/supplier/'+id)
        .then(({data}) => this.form = data)
        .catch()

        if(!User.loggedIn()) {
            this.$router.push({name: '/'})
        }
    },
     data() {
         return {
             form: {
                 name: '',
                 email: '',
                 phone: '',
                 adress: '',
                 photo: '',
                 shopname:'',
                 new_photo: ''
             },
             errors: [],
         }
     },
     methods: {
         supplierUpdate(){
              let id = this.$route.params.id
            axios.patch('/api/supplier/'+id, this.form)
            .then( ()=> {
                this.$router.push({name: 'supplier'})
                Notification.success()
            })
            .catch(error => this.errors = error.response.data.errors)
         },
          onFileSelected(event) {
            let file = event.target.files[0];
            if(file.size > 1048770) {
                Notification.image_validation()
            } else {
                let reader = new FileReader();
                reader.onload = event => {
                    this.form.new_photo = event.target.result
                    // console.log(event.target.result)
                };
                reader.readAsDataURL(file)
            }
            }
     }
}
</script>
