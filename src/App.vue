<template>
  <div class="container h-100">
  <div class="row h-100 justify-content-center align-items-center">
    <form class="pt-3" @submit.prevent="onSubmit">
      <div class="form-group">
        <label for="email">Email
          <input type="email" id="email" class="form-control" :class="{'is-invalid': $v.email.$error}" @blur="$v.email.$touch()" v-model="email">
          <div v-if="!$v.email.required" class="invalid-feedback">
       Email field is required
      </div>
      <div v-if="!$v.email.email" class="invalid-feedback">
       This field should be an email
      </div>
      <div v-if="!$v.email.uniqEmail" class="invalid-feedback">
       This email is already exists
      </div>
        </label>
      </div>

      <div class="form-group">
       <label for="password">Password
            <input type="password"
            id="password"
            class="form-control"
            :class="{'is-invalid':$v.password.$error}"
            @blur="$v.password.$touch()"
            v-model="password">
          <div v-if="!$v.password.minLength" class="invalid-feedback">
            Min length of password is <b>{{$v.password.$params.minLength.min}}</b>. Now it is <b>{{password.length}}</b>
      </div>
     
      </label>
      </div>
    
      <div class="form-group">
       <label for="confirm">Confirm password
            <input type="password"
            id="confirm"
            class="form-control"
            :class="{'is-invalid':$v.confirmPassword.$error}"
            @blur="$v.confirmPassword.$touch()"
            v-model="confirmPassword">
          <div v-if="!$v.confirmPassword.sameAs" class="invalid-feedback">
            Passwords should match
      </div>
     
      </label>
      </div>
      <button class="btn btn-success" type="submit" :disabled="$v.$invalid">Submit</button>
    </form>
  </div>
  </div>
</template>

<script>
import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'
export default {
  data(){
    return{
      email: '',
      password: '',
      confirmPassword: ''
    }
  },
  methods:{
    onSubmit(){
      console.log('Email', this.email)
      console.log('Password', this.password)
    }
  },
  validations:{
    email:{
      required,
      email,
      uniqEmail: function(newEmail){
        return newEmail !=='mail@mail.com'
      }
    },
    password:{
      minLength: minLength(6)
    },
    confirmPassword:{
      sameAs: sameAs((Vue)=>{
        return Vue.password
      })
    }
  }
}
</script>

<style scoped>
form{
  padding: 3%;
  border: 1px solid #ccc;
  border-radius: 5%;
  width: 25%;
  
}
</style>
