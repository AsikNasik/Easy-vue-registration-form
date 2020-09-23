<template>
  <div class="register">
    <h2>Registration</h2>
    <form class="formRegister" @submit.prevent="postData">

    <div class="formGroup" :class="{ 'form-group--error': $v.name.$error }">
      <slot name="name"/>
        <input type="text" class="name filed" v-model.trim="$v.name.$model"/>
    </div>
        <div class="error" v-if="!$v.name.required">Field is required</div>
        <div class="error" v-if="!$v.name.minLength">Name must have at least {{$v.name.$params.minLength.min}} letters.</div>
        <div class="error" v-if="!$v.name.maxLength">Name must have at maximum {{$v.name.$params.maxLength.max}} letters.</div>
        <div class="error" v-if="!$v.name.alpha">Use only alpha characters</div>

    <div class="formGroup" :class="{ 'form-group--error': $v.sername.$error }">
      <slot name="sername"/>
        <input type="text" class="sername filed" v-model.trim="$v.sername.$model"/>
   </div>
        <div class="error" v-if="!$v.sername.required">Field is required</div>
        <div class="error" v-if="!$v.sername.minLength">Name must have at least {{$v.sername.$params.minLength.min}} letters.</div>
        <div class="error" v-if="!$v.sername.maxLength">Name must have at maximum {{$v.sername.$params.maxLength.max}} letters.</div>
        <div class="error" v-if="!$v.sername.alpha">Use only alpha characters</div>

    <slot name="country"/>
      <select class="country filed" v-model="country" required>
        <option class="countryName">Ukraine</option>
        <option class="countryName">Russia</option>
        <option class="countryName">Norway</option>
      </select>

    <slot name="file"/>
      <input type="file" class="file filed" v-on:change="getFileName" required/>
    
      <button type="submit" class="sendData filed" :disabled="submitStatus === 'PENDING'">Send data</button>
      <p class="typo__p" v-if="submitStatus === 'OK'">Thanks for your submission!</p>
      <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
      <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
    </form>
  </div>
</template>

<script>
import { required, minLength, maxLength } from 'vuelidate/lib/validators'
import { helpers } from 'vuelidate/lib/validators'
const alpha = helpers.regex('alpha', /^[a-zA-Z]*$/)

  export default {
    data() {
      return {
        name: null,
        sername: null,
        country: null,
        fileName: null,
        submitStatus: null,
      }
    },
    validations: {
    name: {
      required,
      alpha,
      minLength: minLength(4),
      maxLength: maxLength(10),
    },
    sername: {
      required,
      alpha,
      minLength: minLength(4),
      maxLength: maxLength(10)
    }
  },
    methods: {
      postData(e) {
        this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        this.submitStatus = 'PENDING'
        setTimeout(() => {
          this.submitStatus = 'OK'
        }, 500)
      
        alert('DATA: ' + `\nName - ` + this.name + 
              `\nSername - ` + this.sername + 
              `\nCountry - ` + this.country + 
              `\nFile - ` + this.fileName)
        }
      },

      getFileName(e) {
        this.fileName = e.target.files[0].name;
      },
    },
  }
</script>

<style scoped lang="scss">
  .register {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    text-align: center;
    margin: auto;

    .formRegister, .formGroup {
      display: flex;
      flex-direction: column;

      .error {
        text-align: left;
        font-size: 12px;
        color: red;
      }

      .filed {
        margin: 10px 0 5px 0;
      }

      .label {
        text-align: left;
        margin-top: 20px;
      }

      .sendData {
        margin-top: 30px;
      }
    }
    
  }
</style>
