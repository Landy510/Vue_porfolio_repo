<template>
  <div>
    <div class="vld-parent">
      <loading :active.sync="isLoading"></loading>
    </div>
    <Navbar :product_num="product_length" v-on:increment="CounterCoupute"></Navbar>
    <AlertMessage/>
    <div class="container my-5">
      <div class="d-flex justify-content-center align-items-center">
        <div class="d-flex justify-content-center align-items-center non_finish_bar">
          <div style="height:30px; width:30px; background-color: yellow;"></div>
        </div>
        <span class="progress-connect"></span>
        <div class="non_finish_bar"></div>
        <span class="progress-connect"></span>
        <div class="non_finish_bar"></div>
      </div>
      <div class="d-flex h3 alert alert-primary bg-warning progress_bar mx-auto border-0 text-center justify-content-center align-items-center mb-5 mt-3 rounded-0">
        <p class="m-0 text-dark font-weight-bold">購物車內容</p>
      </div>
      <div class="row">
        <div class="col-lg-9">
          <h2 class="text-center bg-warning py-2">購物車內容</h2>
          <table class="table mt-4">
            <thead>
              <th class="border-bottom-0" width="10"></th>
              <th class="d-none d-md-block border-bottom-0">商品圖片</th>
              <th class="border-bottom-0 table_product" >商品名稱</th>
              <th class="border-bottom-0" >金額</th>
            </thead>
            <tbody>
              <tr v-for="(item, key) in carts" :key="key">
                <td>
                  <button type="button" class="btn btn-outline-danger rounded-0" @click="delProduct(item.id)">
                    <font-awesome-icon :icon="['fas', 'trash-alt']"/>
                  </button>
                </td>
                <td class="d-none d-md-block">
                  <img :src="item.product.imageUrl" width="70px" height="70px" :alt="item.product.title商品">
                </td>
                <td class="h5">
                  <div class="d-flex justify-content-between">
                    <span class="h5 font-weight-bold mb-0">
                      {{ item.product.title }}
                    </span>
                    <div class="input-group w-40 d-none d-md-flex">
                      <div class="input-group-prepend">
                        <button type="button" class="btn btn-grey border border-dark rounded-0" @click="addToCart(item, -1)" :disabled="!clickable">-</button>
                      </div>
                      <input type="text" class="form-control text-center qty h-100" placeholder="0" aria-label="1" aria-describedby="basic-addon1" v-model="item.qty">
                      <div class="input-group-append">
                        <button type="button" class="btn btn-grey border border-dark rounded-0" @click="addToCart(item, 1)" :disabled="!clickable">+</button>
                      </div>
                    </div>
                  </div>
                  <span class="text-muted h5">{{item.product.price| currency }}
                  </span>

                  <div class="input-group w-100 d-flex d-md-none">
                    <div class="input-group-prepend">
                      <button type="button" class="btn btn-grey border border-dark rounded-0" @click="addToCart(item, -1)" :disabled="!clickable">-</button>
                    </div>
                    <input type="text" class="form-control text-center qty" placeholder="0" aria-label="1" aria-describedby="basic-addon1" v-model="item.qty">
                    <div class="input-group-append">
                      <button type="button" class="btn btn-grey border border-dark rounded-0" @click="addToCart(item, 1)" :disabled="!clickable">+</button>
                    </div>
                  </div>
                </td>
                <td class="h5 text-right">
                  {{ item.qty*item.product.price| currency }}
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="col-lg-3">
          <h2 class="bg-light py-2 text-center">應付金額</h2>
          <div class="d-flex justify-content-between">
            <div class="h5">小計</div>
            <div class="h5">{{ total| currency }}</div>
          </div>
          <div class="d-flex justify-content-between border-bottom border-dark mb-2">
            <div class="h5 mb-1">運費</div>
            <div class="h5 mb-1">{{ 0| currency }}</div>
          </div>
          <div class="d-flex justify-content-between">
            <div class="h5 text-danger">總計</div>
            <div class="h5 text-danger">{{ total| currency }}</div>
          </div>
          <div class="d-flex justify-content-between" v-if="final_total!==total && hasCoupon">
            <div class="h5 text-success">折扣價</div>
            <div class="h5 text-success">{{ final_total| currency }}</div>
          </div>
          <div class="input-group mb-3 mt-3">
            <input type="text" class="form-control border border-warning rounded-0" v-model="coupon_code" placeholder="輸入1234折扣碼" aria-label="Recipient's username" aria-describedby="basic-addon2">
            <div class="input-group-append">
              <button class="btn btn-warning rounded-0" type="button" @click="addCouponCode">套用優惠券</button>
            </div>
          </div>
          <router-link to="customer2">
            <button type="button" class="btn btn-warning btn-lg w-100 rounded-0">確認訂單</button>
          </router-link>
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
// import $ from 'jquery'
import AlertMessage from '../../components/AlertMessage.vue'
import Navbar from '../../components/Navbar.vue'
import Footer from '../../components/Footer.vue'
export default {
  name: 'Customer1',
  data () {
    return {
      isLoading: false,
      carts: [],
      total: 0,
      final_total: 0,
      coupon_code: '',
      product_length: 0,
      hasCoupon: false,
      clickable: true
    }
  },
  methods: {
    getList () {
      const vm = this
      vm.isLoading = true
      const url = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`
      vm.$http.get(url).then((response) => {
        if (response.data.data.carts) {
          vm.carts = response.data.data.carts
          vm.product_length = response.data.data.carts.length
          vm.total = response.data.data.total
          vm.final_total = response.data.data.final_total
        }
        vm.isLoading = false
        vm.clickable = true
      })
    },
    delProduct (id) {
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart/${id}`
      vm.isLoading = true
      vm.$http.delete(api).then((response) => {
        if (response.data.success) {
          vm.isLoading = false
          vm.$bus.$emit('messsage:push', response.data.message, 'danger')
        } else {
          vm.isLoading = false
          vm.$bus.$emit('messsage:push', response.data.message, 'danger')
        }
        vm.getList()
      })
    },
    addCouponCode () {
      const vm = this
      vm.isLoading = true
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/coupon`
      const coupon = {
        code: vm.coupon_code
      }
      vm.$http.post(api, { data: coupon }).then((response) => {
        if (response.data.success) {
          vm.hasCoupon = response.data.success
          vm.$bus.$emit('messsage:push', response.data.message, 'success')
          vm.getList()
          vm.isLoading = false
        } else {
          vm.hasCoupon = response.data.success
          vm.$bus.$emit('messsage:push', response.data.message, 'danger')
          vm.getList()
          vm.isLoading = false
        }
      })
      vm.coupon_code = ''
    },
    CounterCoupute (cartTotalLength) {
      this.getList()
      this.product_length = cartTotalLength
    },
    CalQty (data, cal, num) {
      if (cal) {
        data.qty += num
      } else {
        data.qty -= num
      }
    },
    addToCart (data, num) {
      const vm = this
      vm.clickable = false
      data.qty += num
      const target = this.carts.filter(item => item.product_id === data.product_id)
      if (target.length !== 0) {
        const sameItem = target[0]
        const sameItemID = sameItem.id
        const updateQty = data.qty
        vm.$http.delete(`${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart/${sameItemID}`).then(() => {
          console.log('購物車刪除成功')
        }).then(() => {
          const cache = {
            product_id: data.product_id,
            qty: updateQty
          }
          vm.$http.post(`${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`, { data: cache }).then(() => {
            vm.getList()
          })
        })
      } else {
        const cache = {
          product_id: data.product_id,
          qty: data.qty
        }
        vm.$http.post(`${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`, { data: cache }).then(() => {
          vm.isLoading = false
          vm.getList()
        })
      }
    }
  },
  created () {
    this.getList()
  },
  components: {
    AlertMessage,
    Navbar,
    Footer
  }
}
</script>

<style scoped lang="scss">
.progress_bar{
  width: 200px;
  height: 50px;
  z-index: 1;
}
.table_product{
  width:400px;
}
.samll-text{
  font-size:14px;
}
.progress-connect{
  width:200px;
  height:3px;
  background-color:black;
}
.non_finish_bar{
  height:50px;
  width:50px;
  border:4px solid yellow;
}
@media(max-width:760px){
  .table_product{
    width:200px;
  }
}
@media(max-width:680px){
  .progress-connect{
    width:100px;
  }
}
@media(max-width:380px){
  .progress-connect{
    width:50px;
  }
}
</style>
