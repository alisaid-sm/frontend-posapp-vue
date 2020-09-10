<template>
  <div class="container-fluid home">
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
            <b-nav-form class="d-md-block d-none">
              <b-form-input
                size="sm"
                class="mr-sm-2"
                placeholder="Search"
                v-model="search"
                @keyup="searchProducts"
              ></b-form-input>
            </b-nav-form>
            <img
                src="../assets/img/magnifying-glass.png"
                alt
                class="img-responsive img-fluid mr-4 d-md-block d-none"
              />
            <a v-b-toggle.collapse-1 class="d-md-none d-block">
              <img
                src="../assets/img/magnifying-glass.png"
                alt
                class="img-responsive img-fluid mr-4"
              />
            </a>
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
    <div class="row">
      <div class="col-md-1 col-sm-2 col-12 text-center">
        <img
          src="../assets/img/fork.png"
          alt
          class="img-responsive img-fluid d-inline-block d-sm-flex justify-content-center mt-3 mt-sm-5 mb-3 mr-4 mr-sm-0 ml-4 ml-sm-3"
        />
        <router-link to="/history">
          <img
          src="../assets/img/clipboard.png"
          alt
          class="img-responsive img-fluid d-inline-block d-sm-flex justify-content-center mt-3 mt-sm-5 mb-3 mr-4 mr-sm-0 ml-0 ml-sm-3"
        />
        </router-link>

        <a v-b-modal.modal-1>
          <img
            src="../assets/img/add.png"
            alt
            class="img-responsive img-fluid d-inline-block d-sm-flex justify-content-center mt-3 mt-sm-5 mb-3 mr-4 mr-sm-0 ml-0 ml-sm-3"
          />
        </a>
        <a v-b-toggle.sidebar-right
          class="img-fluid d-inline-block d-sm-flex d-lg-none mt-3 mt-sm-5 ml-0 ml-sm-3 button-slider"
        >
          <b-icon icon="cart4" font-scale="3.5" style="vertical-align: middle;"></b-icon>
          <span class="number-cart shadow" style="margin-left: -37px;">0</span>
        </a>
      </div>
      <div class="col-md-7 col-sm-10 col-12 pb-3 content">
        <div class="row">
          <div class="col-12 d-block d-md-none">
          <b-collapse id="collapse-1" class="mt-2">
              <b-form-input
                size="lg"
                placeholder="Search"
                v-model="search"
                @keyup="searchProducts"
              ></b-form-input>
          </b-collapse>
          </div>
          <div class="col-12 mt-3">
            <div class="row justify-content-end">
              <div class="col-md-2 col-3">
                <button class="btn btn-dark" @click="getAll">Get All</button>
              </div>
              <div class="col-md-3 col-4">
                <select v-model="sort" @change="sortProducts" class="form-control form-additem">
                  <option selected value="">Sort by</option>
                  <option value="name">Name</option>
                  <option value="price">Price</option>
                  <option value="date">date</option>
                </select>
              </div>
              <div class="col-md-2 col-3">
                <select v-model="sorttype" @change="sortProducts" class="form-control form-additem">
                  <option selected value=''>ASC</option>
                  <option value="DESC">DESC</option>
                </select>
              </div>
            </div>
          </div>
          <div
            class="col-md-4 col-sm-4 col-6 content1"
            v-for="product in products"
            v-bind:key="product.id_product"
          >
            <Product :product="product" @emitClick = "getAll"/>
          </div>
        </div>
      </div>
      <div class="col-md-4 d-none d-lg-inline-block text-center">
        <img src="../assets/img/food-and-restaurant.png" alt class="img-responsive img-fluid" />
        <p class="font-weight-bold h2 text-empty">Your cart is empty</p>
        <p class="text-please h5">Please add some items from the menu</p>
      </div>
    </div>
    <b-modal id="modal-1" size="lg" centered hide-header hide-footer>
      <div class="m-4">
        <p class="font-weight-bold h3 mb-5">Add Item</p>
        <form @submit.prevent='addProducts'>
          <div class="form-group row font-weight-bold">
            <label for="name" class="col-sm-2 col-form-label col-form-label-lg">Name</label>
            <div class="col-sm-10">
              <input type="text" v-model="form.name" class="form-control form-control-lg form-additem" id="name" required/>
            </div>
          </div>
          <div class="form-group row font-weight-bold">
            <label for="image" class="col-sm-2 col-form-label col-form-label-lg">Image</label>
            <div class="col-sm-10">
              <input type="file" @change="prosesFile($event)" class="form-control form-control-lg form-additem" id="image" required/>
            </div>
          </div>
          <div class="form-group row font-weight-bold">
            <label for="price" class="col-sm-2 col-form-label col-form-label-lg">Price</label>
            <div class="col-sm-5">
              <input type="number" v-model="form.price" class="form-control form-control-lg form-additem" id="price" required/>
            </div>
          </div>
          <div class="form-group row font-weight-bold">
            <label for="category" class="col-sm-2 col-form-label col-form-label-lg">Category</label>
            <div class="col-sm-3">
              <select class="form-control form-control-lg form-additem" v-model="form.category" id="category" required>
                <option selected :value='null'>Category</option>
                <option :value='1'>Food</option>
                <option :value='2'>Drink</option>
              </select>
            </div>
          </div>
          <div class="form-group row float-md-right float-sm-none float-none">
            <div class="col-sm-12">
              <b-button type="reset" class="btn btn-primary button-cancel font-weight-bold mr-3 ml-5" @click="getAll">Cancel</b-button>
              <b-button type="submit" class="btn btn-primary button-add font-weight-bold">Add</b-button>
            </div>
          </div>
        </form>
      </div>
    </b-modal>
    <b-modal id="modal-2" size="lg" centered hide-header hide-footer>
      <div class="m-4">
        <p class="font-weight-bold h3 mb-5">Update Item</p>
        <form @submit.prevent='updateProducts'>
          <div class="form-group row font-weight-bold">
            <label for="name" class="col-sm-2 col-form-label col-form-label-lg">Name</label>
            <div class="col-sm-10">
              <input type="text" v-model="form.name" class="form-control form-control-lg form-additem" id="name" required/>
            </div>
          </div>
          <div class="form-group row font-weight-bold">
            <label for="image" class="col-sm-2 col-form-label col-form-label-lg">Image</label>
            <div class="col-sm-10">
              <input type="file" @change="prosesFile($event)" class="form-control form-control-lg form-additem" id="image" required/>
            </div>
          </div>
          <div class="form-group row font-weight-bold">
            <label for="price" class="col-sm-2 col-form-label col-form-label-lg">Price</label>
            <div class="col-sm-5">
              <input type="number" v-model="form.price" class="form-control form-control-lg form-additem" id="price" required/>
            </div>
          </div>
          <div class="form-group row font-weight-bold">
            <label for="category" class="col-sm-2 col-form-label col-form-label-lg">Category</label>
            <div class="col-sm-3">
              <select class="form-control form-control-lg form-additem" v-model="form.category" id="category" required>
                <option selected :value='null'>Category</option>
                <option :value='1'>Food</option>
                <option :value='2'>Drink</option>
              </select>
            </div>
          </div>
          <div class="form-group row float-md-right float-sm-none float-none">
            <div class="col-sm-12">
              <b-button type="reset" class="btn btn-primary button-cancel font-weight-bold mr-3 ml-5" @click="getAll">Cancel</b-button>
              <b-button type="submit" class="btn btn-primary button-add font-weight-bold">Add</b-button>
            </div>
          </div>
        </form>
      </div>
    </b-modal>
    <b-sidebar id="sidebar-right" bg-variant="white" width="100%" right shadow>
      <div class="row bg-white">
            <div class="col-12 d-flex justify-content-center align-items-center app-header-right">
                <a class="button-slider position-absolute" style="left: 0; top: 0;"><i
                        class="fas fa-times fa-2x"></i></a>
                <p class="font-weight-bold h3">Cart
                    <span class='number-cart'>0</span>
                </p>
            </div>
            <div class="col-12 d-inline-block text-center">
                <img src="../assets/img/food-and-restaurant.png" alt="" class="img-responsive img-fluid">
                <p class="font-weight-bold h2 text-empty">Your cart is empty</p>
                <p class="text-please h5">Please add some items from the menu</p>
            </div>
        </div>
    </b-sidebar>
  </div>
</template>

<script>
// @ is an alias to /src
// import router from '@/router/index.js'
import Product from '@/components/Product.vue'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Product
  },
  data () {
    return {
      form: {
        name: null,
        price: null,
        category: null,
        image: null
      },
      products: null,
      search: null,
      sort: '',
      sorttype: ''
    }
  },
  methods: {
    getAll () {
      axios
        .get('http://localhost:3000/api/v1/product/getall')
        .then((response) => {
        // handle success
          console.log(response.data.data)
          this.products = response.data.data
          // router.push({ path: '/' })
        // console.log(response.data.data[0].name)
        })
        .catch((error) => {
        // handle error
          console.log(error)
        })
    },
    viewProducts (data) {
      this.products = data
    },
    searchProducts () {
      axios
        .get(
          `http://localhost:3000/api/v1/product/getall?search=${this.search}`
        )
        .then((response) => {
          // handle success
          this.viewProducts(response.data.data)
          // console.log(response.data.data[0].name)
        })
        .catch((error) => {
          // handle error
          console.log(error)
        })
    },
    sortProducts () {
      axios
        .get(
          `http://localhost:3000/api/v1/product/getall?sort=${this.sort}&sorttype=${this.sorttype}`
        )
        .then((response) => {
          // handle success
          this.viewProducts(response.data.data)
          // console.log(response.data.data[0].name)
        })
        .catch((error) => {
          // handle error
          console.log(error)
        })
    },
    prosesFile (event) {
      this.form.image = event.target.files[0]
      // console.log(event.target.files[0])
      // console.log(this.form.image)
    },
    addProducts () {
      // alert('Simpan')
      const fd = new FormData()
      fd.append('name', this.form.name)
      fd.append('price', this.form.price)
      fd.append('id_category', this.form.category)
      fd.append('image', this.form.image)
      // console.log(fd)
      axios
        .post(
          'http://localhost:3000/api/v1/product/insert', fd
        )
        .then((response) => {
          // handle success
          this.$toast.success('Sukses Masuk Menu', {
            type: 'success',
            position: 'top-right',
            duration: 3000,
            dismissible: true
          })
          console.log(response)
          // console.log(response.data.data[0].name)
          // update tampilan
          // axios
          //   .get('http://localhost:3000/api/v1/product/getall')
          //   .then((response) => {
          //     // handle success
          //     this.setProducts(response.data.data)
          //     // console.log(response.data.data[0].name)
          //   })
          //   .catch((error) => {
          //     // handle error
          //     console.log(error)
          //   })
        })
        .catch((error) => {
          // handle error
          console.log(error)
        })
    },
    updateGet () {

    },
    updateProduct (id) {
      // console.log(id)
      const fd = new FormData()
      fd.append('name', this.form.name)
      fd.append('price', this.form.price)
      fd.append('id_category', this.form.category)
      fd.append('image', this.form.image)
      axios
        .patch('http://localhost:3000/api/v1/product/updatepatch/' + id)
        .then((response) => {
          this.$toast.error('Sukses Hapus Keranjang', {
            type: 'error',
            position: 'top-right',
            duration: 3000,
            dismissible: true
          })
          console.log(response)
          this.$emit('emitClick')
          // Update Data keranjang
          // axios
          //   .get('http://localhost:3000/api/v1/product/getall')
          //   .then((response) => {
          //     // console.log(response)
          //     this.$emit('update:product', response.data.data)
          //   })
          //   .catch((error) => console.log(error))
        })
        .catch((error) => console.log(error))
    }
  },
  mounted () {
    axios
      .get('http://localhost:3000/api/v1/product/getall')
      .then((response) => {
        // handle success
        this.viewProducts(response.data.data)
        // console.log(response.data.data[0].name)
      })
      .catch((error) => {
        // handle error
        console.log(error)
      })
  },
  updated () {
  }
}
</script>
