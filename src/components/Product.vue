<template>
  <div>
    <div class="" :class="{ select:true, selected: selected }" @click="activeLink">
      <img class=" img-responsive img-fluid img-edit" v-bind:src="'http://localhost:3000/'+ product.image" />
    </div>
    <p class=" h5 mt-1">{{product.name}}</p>
    <p class=" font-weight-bold h5 text-price">Rp. {{product.price}}</p>
    <div class="row">
      <div class="col-6 text-danger text-center"><b-icon-trash font-scale="2" @click="hapusProduct(product.id_product)"></b-icon-trash></div>
      <div class="col-6 text-center">
        <a v-b-modal.modal-2><b-icon font-scale="2" icon="pencil-square"></b-icon></a>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Product',
  data: function () {
    return {
      selected: false
    }
  },
  props: ['product'],
  methods: {
    getAll () {
      this.$emit('emitClick')
    },
    activeLink () {
    // the line below did not work
    // document.getElementsByClassName("active").isActive = false,
      if (this.selected === true) {
        this.selected = false
      } else {
        this.selected = true
      }
    },
    hapusProduct (id) {
      // console.log(id)
      axios
        .delete('http://localhost:3000/api/v1/product/delete/' + id)
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
  }
}
</script>

<style>
</style>
