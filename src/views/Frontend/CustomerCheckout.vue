<template>
  <div class="wrap">
    <div class="vld-parent">
        <loading :active.sync="isLoading"></loading>
    </div>
    <Navbar></Navbar>
    <AlertMessage/>
    <div class="container my-5 flex-grow-1">
      <div class="d-flex justify-content-center align-items-center">
        <div class="d-flex justify-content-center align-items-center finish_bar"></div>
        <span class="progress-connect"></span>
        <div class="d-flex justify-content-center align-items-center finish_bar"></div>
        <span class="progress-connect"></span>
        <div class="d-flex justify-content-center align-items-center non_finish_bar">
          <div style="height:30px; width:30px; background-color: yellow;"></div>
        </div>
      </div>
      <div class="d-flex h3 alert alert-primary bg-warning progress_bar mx-auto border-0 text-center justify-content-center align-items-center mb-5 mt-3 rounded-0">
        <p class="m-0 text-dark font-weight-bold">資料確認</p>
      </div>
      <div class="row">
        <div class="col-md-6">
          <div class="CheckoutPicture bg-cover"></div>
        </div>
        <div class="col-md-6">
          <h4 class="order-title mb-3">
            <font-awesome-icon class="mr-2" :icon="['fas', 'flag']" />訂單成立
          </h4>
          <div class="px-0 px-sm-4">
            <table class="table">
              <tbody>
                <tr>
                  <td>訂單ID</td>
                  <td>{{ order.id }}</td>
                </tr>
                <tr>
                  <td>姓名</td>
                  <td>{{ order.user.name }}</td>
                </tr>
                <tr>
                  <td>Email</td>
                  <td>{{ order.user.email }}</td>
                </tr>
                <tr>
                  <td>電話</td>
                  <td>{{ order.user.tel }}</td>
                </tr>
                <tr>
                  <td>應付金額</td>
                  <td>{{ order.total | currency}}</td>
                </tr>
                <tr>
                  <td>付款狀態</td>
                  <td v-if="!order.is_paid" class="text-danger font-weight-bold">未付</td>
                  <td v-else class="text-success font-weight-bold">已完成付款</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="text-right" v-if="order.is_paid === false">
            <router-link to="/CustomerOrder/CustomerFinish">
              <button class="btn btn-warning w-100 rounded-0" type="button" @click="payOrder">確認付款去</button>
            </router-link>
          </div>
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
import AlertMessage from '../../components/AlertMessage.vue'
import Navbar from '../../components/Navbar.vue'
import Footer from '../../components/Footer.vue'
export default {
  name: 'Customer1',
  data () {
    return {
      isLoading: false,
      orderId: '',
      order: {
        user: {
          email: ''
        }
      }
    }
  },
  methods: {
    getList () {
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/order/${vm.orderId}`
      vm.$http.get(api).then((response) => {
        vm.order = response.data.order
      })
    },
    CounterCoupute (cartTotalLength) {
      this.getList()
      this.product_length = cartTotalLength
    },
    payOrder () {
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/pay/${vm.orderId}`
      vm.isLoading = true
      vm.$http.post(api).then((response) => {
        if (response.data.success) {
          vm.$router.push(`/customerOrder/CustomerFinish/${vm.orderId}`)
          vm.isLoading = false
        }
      })
    }
  },
  created () {
    this.orderId = this.$route.params.orderId
    this.getList()
  },
  components: {
    AlertMessage,
    Navbar,
    Footer
  }
}
</script>

<style scoped>
.cart_content_btn:hover{
  cursor:pointer;
}
.confirm_btn{
  width:50%;
}
.cart_content{
  display: none;
}
.arrow{
  transform: rotate(0deg);
  transition: all .5s;
}
.arrow.active{
  transform: rotate(180deg);
  transition: all .5s;
}
.CheckoutPicture{
  background-image: url(https://upload.cc/i1/2020/12/26/5QczGW.jpg);
  position:absolute;
  left:0;
  right:0;
  top:0;
  bottom:0;
}
.order-title {
  position:relative;
  width:30%;
  text-align:center;
  margin:0 auto 0 auto;
}
.order-title:before {
  content: "";
  display: block;
  height: 40%;
  width: 100%;
  background-color: rgba(236, 230, 64, .6);
  position: absolute;
  bottom: 0;
  left: 0;
  z-index: -1;
}
/*---*/
.progress_bar{
  width: 200px;
  height: 50px;
  z-index: 1;
}
.progress-connect{
  width:200px;
  height:3px;
  background-color:black;
}
.finish_bar{
  height:50px;
  width:50px;
  background-color: yellow;
}
.non_finish_bar{
  height:50px;
  width:50px;
  border:4px solid yellow;
}
@media(max-width:820px) {
  .order-title {
    width: 40%;
  }
}
@media(max-width:680px){
  .confirm_btn{
    width:100%;
  }
  .progress-connect{
    width:100px;
  }
}
@media(max-width:380px){
  .progress-connect{
    width:50px;
  }
}
@media(max-width:360px){
  .order-title {
    width: 50%;
  }
}
</style>
