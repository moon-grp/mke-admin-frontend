<template>
  <div>
    <div id="boxSpace">
      <v-row class="justify-center mb-8">
        <div class="fnt text-capitalize">
          sign in to admin dashboard<span class="coll">.</span>
        </div>
      </v-row>
      <v-row class="justify-center">
        <div class="divl">
          <v-text-field
            outlined
            label="Password"
            prepend-inner-icon="mdi-key"
            class="text-capitalize tf"
            color="#6C63FF"
            v-model="password"
            :error-messages="passwordErros"
            @input="$v.password.$touch()"
            @blur="$v.password.$touch()"
            type="password"
          ></v-text-field>
        </div>
      </v-row>
      <v-row class="justify-center">
        <div>
          <v-btn
            color="#6C63FF"
            class="fnt-p trs"
            outlined
            dark
            @click="signIn"
            :loading="loading"
          >
            sign in
          </v-btn>
        </div>
      </v-row>
    </div>
    <v-snackbar v-model="snackbar" :timeout="timeout" color="success">
      {{ msg }}
    </v-snackbar>
    <v-snackbar v-model="snackbarErr" :timeout="timeout" color="error">
      {{ msg }}
    </v-snackbar>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
export default {
  layout: 'admin',
  mixins: [validationMixin],

  validations: {
    password: { required, minLength: minLength(8) },
  },
  data() {
    return {
      password: null,
      loading: false,
      snackbar: false,
      snackbarErr: false,
      timeout: 7000,
      msg: '',
    }
  },
  methods: {
    async signIn() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        this.loading = true
        try {
          const res = await this.$axios.$post(
            'https://mrkayenterprise.herokuapp.com/api/v1/admin/signin',
            {
              password: this.password,
            }
          )
          console.log(res)
          // localStorage.setItem('token', JSON.stringify(res.token))
          this.$cookies.set('token', res.access_token, {
            path: '/',
            maxAge: 60 * 60 * 24 * 7,
          })
          this.msg = res.message
          this.snackbar = true
          this.$router.push({ name: 'dashboard' })
          this.loading = false
        } catch (error) {
          console.log(error)
          this.msg = error.response.data.detail
          this.snackbarErr = true
          this.loading = false
        } 

      }
    },
  },
  computed: {
    passwordErros() {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      !this.$v.password.minLength &&
        errors.push('Password must be at most 8 characters long')
      !this.$v.password.required && errors.push('Your password is required.')
      return errors
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');

#boxSpace {
  margin-top: 5%;
  margin-bottom: 10%;
}

.tf {
  border-radius: 10px;
}

.divl {
  width: 50vw;
}

.fnt {
  font-family: 'Poppins', sans-serif;
  font-size: 50px;
}

.coll {
  color: #6c63ff;
  font-size: 70px;
}
</style>
