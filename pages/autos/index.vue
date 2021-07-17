<template>
  <div>
    <v-row class="mt-4">
      <v-col
        sm="4"
        xl="2"
        lg="3"
        md="3"
        xs="6"
        v-for="item in getProducts"
        :key="item._id.$oid"
      >
        <v-card max-height="500">
          <v-img
            :src="item.mediaUrl"
            class="white--text align-end"
            gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
            height="200px"
            contain
          >
            <v-card-title class="text-capitalize">{{
              item.carname
            }}</v-card-title>
          </v-img>

          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title>Description:</v-list-item-title>
              <v-list-item-subtitle class="text-capitalize">{{
                item.description
              }}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title>Availability:</v-list-item-title>
              <v-list-item-subtitle class="text-capitalize">{{
                item.available
              }}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title>Price:</v-list-item-title>
              <v-list-item-subtitle class="text-capitalize"
                >₦ {{ item.carprice }}</v-list-item-subtitle
              >
            </v-list-item-content>
          </v-list-item>

          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title>Slash Price:</v-list-item-title>
              <v-list-item-subtitle class="text-capitalize"
                >₦ {{ item.commission }}</v-list-item-subtitle
              >
            </v-list-item-content>
          </v-list-item>

          <v-card-actions>
            <v-spacer></v-spacer>

            <v-btn
              icon
              @click="
                getProductDetails(
                  item._id.$oid,
                  item.carname,
                  item.carprice,
                  item.commission,
                  item.description,
                  item.location
                )
              "
            >
              <v-icon>mdi-border-color</v-icon>
            </v-btn>

            <v-btn icon @click="getProductId(item._id.$oid)">
              <v-icon>mdi-delete</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <!-- dialogs -->
    <v-dialog v-model="dialogDelete" persistent max-width="290">
      <v-card>
        <v-card-title class="caption">
          Sure you want to delete <b> </b> ?
        </v-card-title>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" text @click="deleteProduct()">
            yes
          </v-btn>
          <v-btn color="red darken-1" text @click="dialogDelete = false">
            no
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogEdit" persistent fullscreen hide-overlay>
      <v-card class="mx-auto my-4" min-width="300">
        <v-toolbar dark color="primary">
          <v-btn icon dark @click="dialogEdit = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Edit ({{ productName }})</v-toolbar-title>
        </v-toolbar>
        <v-text-field
          label="car name"
          outlined
          class="mx-8 mt-8 text-capitalize"
          color="#6C63FF"
          v-model="productName"
        ></v-text-field>

        <v-textarea
          outlined
          name="input-7-4"
          label="description"
          class="mx-8 mt-2 text-capitalize"
          color="#6C63FF"
          v-model="productDescription"
        ></v-textarea>

        <v-text-field
          label="price"
          outlined
          class="mx-8 mt-2 text-capitalize"
          color="#6C63FF"
          type="number"
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
            update product
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- pop up -->
    <v-snackbar v-model="snackbar" :timeout="timeout" color="success">
      {{ msg }}
    </v-snackbar>
    <v-snackbar v-model="snackbarErr" :timeout="timeout" color="error">
      {{ msg }}
    </v-snackbar>
  </div>
</template>

<script>
export default {
  layout: 'autos',
  data() {
    return {
      getProducts: '',
      dialogEdit: false,
      dialogDelete: false,
      snackbar: false,
      snackbarErr: false,
      timeout: 7000,
      msg: '',
      pid: '',
      loading: false,
      productPrice: null,
      productName: null,
      productDescription: null,
      productSlashPrice: null,
      productImage: '',
      available: '',
      productPriceE: null,
      productNameE: null,
      productDescriptionE: null,
      productSlashPriceE: null,
      productImage: '',
      location:"",
      file: [],
    }
  },
/*async asyncData({ $axios }) {
    const getProducts = await $axios.$get(
      'https://mrkayenterprise.herokuapp.com/api/v1/admin/viewproducts'
    )

    console.log(getProducts)

    return { getProducts }
  }, */
   created() {
    this.product()
  },
  methods: {
     async product() {
      this.loading = true
      try {
        const res = await this.$axios.$get(
               'https://mrkayenterprise.herokuapp.com/api/v1/admin/viewcars'
        )
        console.log(res)
        this.getProducts = res.cars
        this.loading = false
      } catch (error) {
        console.log(error)
      }
    },
    async deleteProduct() {
      try {
        this.dialogDelete = false
        const res = await this.$axios.$delete(
          `https://mrkayenterprise.herokuapp.com/api/v1/admin/deletecar/${this.pid}`
        )
        //  console.log(res)
        this.msg = 'Product deleted succesfully...'
        this.snackbar = true
        location.reload()
      } catch (error) {
        this.dialogDelete = false
        // console.log(error.response)
        this.msg = error.response.data
        this.snackbarErr = true
      }
    },
    getProductId(id) {
      this.pid = id
      //console.log(this.pid)
      this.dialogDelete = true
    },
    getProductDetails(id, name, price, slash, description, location) {
      this.pid = id
      this.productPrice = price
      this.productName = name
      this.productDescription = description
      this.productSlashPrice = slash
      this.location = location

      this.dialogEdit = true
    },
    async uploadProduct() {
      const fd = new FormData()
      fd.append('file', this.file)
      fd.append('carprice', this.productPrice)
      fd.append('carname', this.productName)
      const available = true
      fd.append('available', available)
      fd.append('commission', this.productSlashPrice)
       fd.append('location', this.location)
      fd.append('description', this.productDescription)

      this.loading = true
      try {
        const res = await this.$axios.$post(
          `https://mrkayenterprise.herokuapp.com/api/v1/admin/updatedcar/${this.pid}`,

          fd
        )
        console.log(res)
        this.msg = 'Product Updated succesfully...'
        this.snackbar = true
        this.loading = false
        location.reload()
      } catch (error) {
        console.log(error.response)
        this.msg = error.response.data
        this.snackbarErr = true
        this.loading = false
      }
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');

body {
  font-family: 'Poppins', sans-serif;
  font-size: 10px;
}
</style>
