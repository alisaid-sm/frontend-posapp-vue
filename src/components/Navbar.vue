<template>
  <div class="row">
    <div class="col-md-8 col-sm-12 col-12 app-header">
      <div class="row h-100 align-items-center">
        <div class="col-md-2 col-sm-2 col-2 d-flex">
          <img src="../assets/img/menu (1).png" alt class="img-responsive img-fluid ml-3" />
        </div>
        <div class="col-md-8 col-sm-8 col-8 d-flex justify-content-center">
          <p class="font-weight-bold h2">Food Items</p>
        </div>
        <div class="col-md-2 col-sm-2 col-2 d-flex justify-content-end">
          <b-nav-form>
            <b-form-input size="sm" class="mr-sm-2" placeholder="Search" v-model="search" @keyup="searchProducts"></b-form-input>
          </b-nav-form>
          <img src="../assets/img/magnifying-glass.png" alt class="img-responsive img-fluid mr-4" />
        </div>
      </div>
    </div>
    <div
      class="col-md-4 d-none d-lg-flex justify-content-center align-items-center app-header-right"
    >
      <p class="font-weight-bold h3">
        Cart
        <span class="number-cart">0</span>
      </p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Navbar',
  data () {
    return {
      search: ''
    }
  },
  methods: {
    searchProducts () {
      axios.get('http://localhost:3000/api/v1/product/getall?search=' + this.search)
        .then((response) => {
        // handle success
          this.setProducts(response.data.data)
          // console.log(response.data.data[0].name)
        })
        .catch((error) => {
        // handle error
          console.log(error)
        })
    }
  },
  mounted () {
    axios.get('http://localhost:3000/api/v1/product/getall')
      .then((response) => {
        // handle success
        this.setProducts(response.data.data)
        // console.log(response.data.data[0].name)
      })
      .catch((error) => {
        // handle error
        console.log(error)
      })
  }
}
</script>

<style>
</style>
