<template>
  <div>
    <v-row class="justify-center mt-6 fnt mb-8">
      <v-card class="mx-auto" min-width="500">
        <v-text-field
          label="car name"
          outlined
          class="mx-8 mt-8 text-capitalize"
          color="#6C63FF"
          v-model="productName"
          :error-messages="pnError"
          @input="$v.productName.$touch()"
          @blur="$v.productName.$touch()"
        ></v-text-field>

        <v-textarea
          outlined
          name="input-7-4"
          label="car description"
          class="mx-8 mt-2 text-capitalize"
          color="#6C63FF"
          v-model="productDescription"
          :error-messages="pdError"
          @input="$v.productDescription.$touch()"
          @blur="$v.productDescription.$touch()"
        ></v-textarea>

        <v-text-field
          label="car price"
          outlined
          class="mx-8 mt-2 text-capitalize"
          color="#6C63FF"
          type="number"
          :error-messages="ppError"
          @input="$v.productPrice.$touch()"
          @blur="$v.productPrice.$touch()"
          prefix="₦"
          v-model="productPrice"
        ></v-text-field>

        <v-text-field
          label="commission"
          outlined
          class="mx-8 mt-2 text-capitalize"
          color="#6C63FF"
          type="number"
          v-model="productSlashPrice"
          prefix="₦"
        ></v-text-field>

        <v-text-field
          label="location"
          outlined
          class="mx-8 mt-2 text-capitalize"
          color="#6C63FF"
          v-model="location"
          :error-messages="locationError"
          @input="$v.location.$touch()"
          @blur="$v.location.$touch()"
        ></v-text-field>

        <v-file-input
          accept="image/*"
          label="Product image"
          class="mx-8 mt-2"
          color="#6C63FF"
          ref="sI"
          v-model="file"
        ></v-file-input>

        <v-card-actions>
          <v-btn
            text
            dark
            color="#6C63FF"
            @click="uploadProduct"
            :loading="loading"
            class="ml-6 mt-2"
          >
            upload product
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-row>
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
  layout: 'autos',
  mixins: [validationMixin],

  validations: {
    productName: { required },
    productDescription: { required },
    productPrice: { required },
    location:{required}
  },
  data() {
    return {
      productPrice: null,
      productName: null,
      productDescription: null,
      productSlashPrice: null,
      location:null,
      productImage: '',
      file: [],
      snackbar: false,
      snackbarErr: false,
      timeout: 7000,
      msg: '',
      loading: false,
    }
  },
  computed: {
    pnError() {
      const errors = []
      if (!this.$v.productName.$dirty) return errors
      !this.$v.productName.required && errors.push('product name is required')
      return errors
    },
    pdError() {
      const errors = []
      if (!this.$v.productDescription.$dirty) return errors
      !this.$v.productDescription.required &&
        errors.push('product description is required')
      return errors
    },
    ppError() {
      const errors = []
      if (!this.$v.productPrice.$dirty) return errors
      !this.$v.productPrice.required && errors.push('product price is required')
      return errors
    },
      locationError() {
      const errors = []
      if (!this.$v.location.$dirty) return errors
      !this.$v.location.required && errors.push('location is required')
      return errors
    },
  },
  methods: {
    resetValues() {
      this.productName = ''
      this.productPrice = ''
      this.productSlashPrice = ''
      this.productDescription = ''
      this.location = ''
    },
    async uploadProduct() {
      const fd = new FormData()
      fd.append('file', this.file)
      fd.append('carprice', this.productPrice)
      fd.append('carname', this.productName)
      const available = true
      fd.append('available', available)
      fd.append('commission', this.productSlashPrice)
      fd.append('description', this.productDescription)
      fd.append('location', this.location)

      this.$v.$touch()
      if (!this.$v.$invalid) {
        this.loading = true
        try {
          const res = await this.$axios.$post(
            'https://mrkayenterprise.herokuapp.com/api/v1/admin/createpost',

            fd
          )
          console.log(res)
          this.msg = 'Product Uploaded succesfully...'
          this.snackbar = true
          this.loading = false
          this.resetValues()
        } catch (error) {
          console.log(error.response)
          this.msg = error.response.data
          this.snackbarErr = true
          this.loading = false
        }
      }
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');

.fnt {
  font-family: 'Poppins', sans-serif;
  font-size: 10px;
}
</style>
