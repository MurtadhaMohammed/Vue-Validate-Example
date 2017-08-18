<template>
  <div id="app">
    <form @submit.prevent="validateBeforeSubmit" v-if="!formSubmitted">

      <h1>Form Validation</h1>
      <hr>
      <button @click="changeLocale" type="button" class="form-control btn btn-success">Change Language To {{  nextLocale }}</button>
      <hr>
      <div class="form-group" :class="{'has-error': errors.has('name') }">
        <label class="control-label" for="name">Name</label>
        <input name="name" v-model="name" v-validate.initial="name" data-vv-as="name" data-rules="required|alpha|min:3" class="form-control"
          type="text" placeholder="Full Name">
        <p class="text-danger" v-if="errors.has('name')">{{ errors.first('name') }}</p>
      </div>
      <div class="form-group" :class="{'has-error': errors.has('email') }">
        <label class="control-label" for="email">Email</label>
        <input name="email" v-model="email" v-validate.initial="email" data-rules="required|email" class="form-control" type="email"
          placeholder="Email">
        <p class="text-danger" v-if="errors.has('email')">{{ errors.first('email') }}</p>
      </div>
      <button class="btn btn-primary btn-block" type="submit">Submit</button>
    </form>
    <div v-else>
      <h1 class="submitted">Form submitted successfully!</h1>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import VeeValidate, { Validator } from 'vee-validate';
  import arabic from 'vee-validate/dist/locale/ar';

  Vue.use(VeeValidate);


  export default {

    data() {
      return {
        email: '',
        name: '',
        url: '',
        secret: '',
        formSubmitted: false,
        language: 'en',
      }
    },
    computed: {
      nextLocale() {
        return this.language === 'en' ? 'Arabic' : 'English';
      }
    },
    methods: {
      changeLocale() {

        if (this.language == 'ar') {
          this.language = 'en';
          this.$validator.setLocale(this.language);
        } else {
          this.language = 'ar';
          this.$validator.setLocale(this.language);

        }
      },
      validateBeforeSubmit(e) {
        this.$validator.validateAll();

        if (!this.errors.any()) {
          this.submitForm()
        }
      },
      submitForm() {
        this.formSubmitted = true
      }
    },
    created() {
      this.$validator.updateDictionary({
        ar: {
          messages: arabic,
          attributes: {
            email: 'البريد الاليكتروني',
            name: 'الاسم'
          }
        }
      });
    },
  }

</script>

<style>
  body {
    font-family: Helvetica, sans-serif;
  }

  .container {
    width: 500px;
  }

  h1 {
    text-align: center
  }


  .submitted {
    color: #4fc08d;
  }
</style>