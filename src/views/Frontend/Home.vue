<template>
  <div>
    <div class="vld-parent">
      <loading :active.sync="isLoading"></loading>
    </div>
    <Navbar :product_num="product_length" v-on:increment="CounterCoupute" ></Navbar>
    <Introbanner :introImage="image_website" :introImage_1="image_website1" :introImage_2="image_website2"></Introbanner>
    <AlertMessage/>
    <div class="container main-content my-3">
      <div class="row my-5">
        <div class="col-12 text-center">
          <small class="text-muted">教練推薦課程</small>
          <p class="h2 mb-4 font-weight-normal">Recommended</p>
          <carousel :autoplay="true" :loop="true" :paginationEnabled="false" :perPageCustom="[[320, 1], [560, 1], [768, 2], [1024, 3]]">
            <slide v-for="(item, index) in aerobicArray" :key="index">
              <div class="card h-100 border-0 mr-2">
                <img class="card-img-top h-50" :src="item.imageUrl" :alt="`${item.title}課程`">
                <div class="card-body pt-1">
                  <p class="card-title font-weight-bold mb-1 h5">{{ item.title }}</p>
                  <p class="card-title medium-text text-muted">{{ item.description }}</p>
                  <div class="d-flex justify-content-between align-items-end">
                    <small class="card-text text-muted small-text" v-if="item.origin_price!==item.price">原本售價<del>{{ item.origin_price| currency }}</del></small>
                    <strong class="card-text text-muted ml-auto">現在售價 <span class="h4 text-danger">{{ item.price | currency }}</span></strong>
                  </div>
                  <button class="btn btn-outline-dark rounded-0 btn-md-lg rounded-0 w-100" type="button" @click="getProduct(item.id)">前去課程介紹</button>
                </div>
              </div>
            </slide>
          </carousel>
        </div>
      </div>
      <div class="row flex-column mb-5">
        <div class="col-12 text-center">
          <small class="text-muted">課程選擇</small>
          <p class="h2 mb-4 font-weight-normal">Lecture</p>
          <div class="list-group flex-row lecture_option mx-auto mb-3" id="list-tab" role="tablist">
            <a class="list-group-item list-group-item-action active border-0 text-dark rounded-0" id="list-home-list" data-toggle="list" href="#list-home" role="tab" aria-controls="home">All</a>
            <a class="list-group-item list-group-item-action border-0 text-dark rounded-0" id="list-Workout-list" data-toggle="list" href="#list-Workout" role="tab" aria-controls="Workout">Workout</a>
            <a class="list-group-item list-group-item-action border-0 text-dark rounded-0" id="list-women-list" data-toggle="list" href="#list-women" role="tab" aria-controls="profile">Aerobic</a>
          </div>
          <div class="tab-content mb-3" id="nav-tabContent">
            <div class="tab-pane fade show active" id="list-home" role="tabpanel" aria-labelledby="list-home-list">
              <div class="row">
                <div class="col-sm-6 col-lg-4" v-for="(item) in products" :key="item.id">
                  <div class="card h-100 border-0 mb-5">
                    <img class="card-img-top h-60" :src="item.imageUrl" :alt="`${item.title}課程`">
                    <div class="card-body p-0">
                      <p class="card-title font-weight-bold mb-1 h5">{{ item.title }}</p>
                      <p class="card-title medium-text text-muted">{{ item.description }}</p>
                      <div class="d-flex justify-content-between align-items-end">
                        <small class="card-text text-muted small-text" v-if="item.origin_price!==item.price">原本售價<del>{{ item.origin_price| currency }}</del></small>
                        <strong class="card-text text-muted ml-auto">現在售價 <span class="h4 text-danger">{{ item.price | currency }}</span></strong>
                      </div>
                      <button type="button" class="btn btn-outline-dark rounded-0 btn-md-lg rounded-0 w-100" @click="getDetail(item.id)"><font-awesome-icon :icon="['fas', 'cart-arrow-down']" class="mr-2" />加入購物車</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="tab-pane fade" id="list-Workout" role="tabpanel" aria-labelledby="list-Workout-list">
                <div class="row">
                  <div class="col-sm-6 col-lg-4 mb-2" v-for="(item, index) in workoutArray" :key="index">
                    <div class="card h-100 border-0">
                      <img class="card-img-top h-60" :src="item.imageUrl" :alt="`${item.title}課程`">
                      <div class="card-body p-0">
                        <p class="card-title font-weight-bold mb-1 h5">{{ item.title }}</p>
                        <p class="card-title medium-text text-muted">{{ item.description }}</p>
                        <div class="d-flex justify-content-between align-items-end">
                          <small class="card-text text-muted small-text" v-if="item.origin_price!==item.price">原本售價<del>{{ item.origin_price }}</del></small>
                          <strong class="card-text text-muted ml-auto">現在售價 <span class="h4 text-danger">{{ item.price }}</span></strong>
                        </div>
                        <button type="button" class="btn btn-outline-dark rounded-0 btn-md-lg rounded-0 w-100" @click="getDetail(item.id)"><font-awesome-icon :icon="['fas', 'cart-arrow-down']" class="mr-2" />加入購物車</button>
                      </div>
                    </div>
                  </div>
                </div>
            </div>
            <div class="tab-pane fade" id="list-women" role="tabpanel" aria-labelledby="list-women-list">
                <div class="row">
                  <div class="col-sm-6 col-lg-4 mb-2" v-for="(item, index) in aerobicArray" :key="index">
                    <div class="card h-100 border-0">
                      <img class="card-img-top h-60" :src="item.imageUrl" :alt="`${item.title}課程`">
                      <div class="card-body p-0">
                        <p class="card-title font-weight-bold mb-1 h5">{{ item.title }}</p>
                        <p class="card-title medium-text text-muted">{{ item.description }}</p>
                        <div class="d-flex justify-content-between align-items-end">
                          <small class="card-text text-muted small-text" v-if="item.origin_price!==item.price">原本售價<del>{{ item.origin_price }}</del></small>
                          <strong class="card-text text-muted ml-auto">現在售價 <span class="h4 text-danger">{{ item.price }}</span></strong>
                        </div>
                        <button type="button" class="btn btn-outline-dark rounded-0 btn-md-lg rounded-0 w-100" @click="getDetail(item.id)"><font-awesome-icon :icon="['fas', 'cart-arrow-down']" class="mr-2" />加入購物車</button>
                      </div>
                    </div>
                  </div>
                </div>
            </div>
          </div>
          <router-link class="btn btn-outline-dark btn-lg d-block rounded-0 mt-3 goTolecture_btn" to="/lecture/LectureProduct">前往課程頁一覽</router-link>
        </div>
      </div>
      <div class="row text-center">
        <div class="col-12">
          <small class="text-muted small-text">大家都怎麼推薦</small>
          <p class="h2 mb-4 font-weight-normal">How is Everyone feeling</p>
          <div class="row">
            <div class="col-md-4">
              <div class="card h-100 border-0 text-justify">
                <img class="card-img-top mx-auto" src="https://randomuser.me/api/portraits/women/10.jpg" alt="`客戶評價頭像`" style="width:100px; height:100px; border-radius:50%;">
                <div class="card-body">
                  <p class="card-text mb-0 text-center font-weight-bold">來自深淵的女子</p>
                  <small class="card-text text-muted medium-text">Berserker Fitness真的解救了我多年來苦惱的肉肉，看著肉肉越來越少，褲子越來越鬆，我的身心靈彷彿就得到了淨化，
                  強烈推薦你/妳來Berserker體驗練到腿軟的滋味。</small>
                </div>
              </div>
            </div>
            <div class="col-md-4">
              <div class="card h-100 border-0 text-justify">
                <img class="card-img-top mx-auto" src="https://randomuser.me/api/portraits/men/3.jpg" alt="客戶評價頭像" style="width:100px; height:100px; border-radius:50%;">
                <div class="card-body">
                  <p class="card-text mb-0 text-center font-weight-bold">基隆的嗨先生</p>
                  <small class="card-text text-muted medium-text">Berserker Fitness無話可說無可救藥的<strong class="h4">就是讚!!!</strong></small>
                </div>
              </div>
            </div>
            <div class="col-md-4">
              <div class="card h-100 border-0 text-justify">
                <img class="card-img-top mx-auto" src="https://randomuser.me/api/portraits/men/90.jpg" alt="客戶評價頭像" style="width:100px; height:100px; border-radius:50%;">
                <div class="card-body">
                  <p class="card-text mb-0 text-center font-weight-bold">來自日本的小林先生</p>
                  <small class="card-text text-muted medium-text"><strong class="h4">紅豆泥!!!</strong>你/妳還沒有加入Berserker Fitness的健身房，妳是不是阿搭麻
                  控估力</small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="modal fade" id="productModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content border-0">
          <div class="modal-header text-dark">
            <h3 class="modal-title" id="exampleModalLabel" v-html="product_detail.title"></h3>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="card border-0">
              <img class="card-img-top rounded-0" :src= "product_detail.imageUrl" :alt="`${product_detail.title}課程圖片`">
              <div class="card-body">
                <div v-html="product_detail.content"></div>
                <div class="d-flex justify-content-between align-items-end">
                  <del class="origin_price_text">原價{{ product_detail.origin_price }}元</del>
                  <strong class="special_price_text">現在只要<span class="text-danger">{{ product_detail.price }}</span>元</strong>
                </div>
                <div class="form-group mt-3">
                  <div class="form-group">
                    <select class="form-control form-control-lg rounded-0" aria-label="Default select example" v-model="lecturenum">
                      <option value='' disabled>請選擇數量</option>
                      <option :value="num" v-for="num in 10" :key="num">選購{{ num }} {{ product_detail.unit }}</option>
                    </select>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <span class="h4 text-muted">小計 {{ product_detail.price*lecturenum }} 元</span>
            <button type="button" class="btn btn-warning rounded-0" @click="addToCart(product_detail, lecturenum)">加到購物車</button>
          </div>
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
import Navbar from '../../components/Navbar.vue'
import Introbanner from '../../components/Introbanner.vue'
import AlertMessage from '../../components/AlertMessage.vue'
import Footer from '../../components/Footer.vue'
import $ from 'jquery'
import { Carousel, Slide } from 'vue-carousel'

export default {
  name: 'Home',
  data () {
    return {
      lecturenum: '',
      products: [],
      product_detail: [],
      searchText: '',
      categories: [],
      isLoading: false,
      status: {
        loadingItem: ''
      },
      carts: [],
      product_length: 0,
      image_website: 'https://upload.cc/i1/2021/03/10/0QKA7j.jpg',
      image_website1: 'https://upload.cc/i1/2021/02/18/cyrh8S.jpg',
      image_website2: 'https://upload.cc/i1/2020/12/26/5QczGW.jpg'
    }
  },
  methods: {
    getProducts (page = 1) {
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/products/all`
      vm.isLoading = true
      vm.$http.get(api).then((response) => {
        if (response.data.success) {
          vm.products = response.data.products
        }
        vm.isLoading = false
      })
    },
    getProduct (id) {
      this.$router.push(`/lecture/${id}`)
    },
    getDetail (id) {
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/product/${id}`
      vm.status.loadingItem = id
      vm.$http.get(api).then((response) => {
        $('#productModal').modal('show')
        vm.product_detail = response.data.product
        vm.status.loadingItem = ''
      })
    },
    addToCart (data, qty = 1) {
      const vm = this
      vm.isLoading = true
      const url = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`
      vm.$http.get(url).then((response) => {
        if (response.data.data.carts) {
          vm.carts = response.data.data.carts
          vm.product_length = response.data.data.carts.length
        }
      }).then(() => {
        const target = vm.carts.filter(item => item.product.title === data.title)
        if (target.length !== 0) {
          const sameItem = target[0]
          const sameItemID = sameItem.id
          const updateQty = sameItem.qty + qty
          vm.$http.delete(`${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart/${sameItemID}`).then(() => {
            console.log('購物車刪除成功')
          }).then(() => {
            const cache = {
              product_id: sameItem.product_id,
              qty: updateQty
            }
            vm.$http.post(`${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`, { data: cache }).then(() => {
              vm.$bus.$emit('messsage:push', '已加入購物車', 'success')
              $('#productModal').modal('hide')
              vm.getCart()
              vm.isLoading = false
            })
          })
        } else {
          const cache = {
            product_id: data.id,
            qty: qty
          }
          vm.$http.post(`${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`, { data: cache }).then((response) => {
            $('#productModal').modal('hide')
            vm.$bus.$emit('messsage:push', '已加入購物車', 'success')
            vm.getCart()
            vm.isLoading = false
          })
        }
      })
      vm.lecturenum = ''
    },
    getCart () {
      const vm = this
      vm.isLoading = true
      const url = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`
      vm.$http.get(url).then((response) => {
        if (response.data.data.carts) {
          vm.carts = response.data.data.carts
          vm.product_length = response.data.data.carts.length
        }
        vm.isLoading = false
      })
    },
    activeCart () {
      $('.cart_list_cover').fadeToggle()
    },
    CounterCoupute (cartTotalLength) {
      this.product_length = cartTotalLength
    }
  },
  created () {
    this.getProducts()
    this.getCart()
  },
  components: {
    AlertMessage,
    Introbanner,
    Navbar,
    Carousel,
    Slide,
    Footer
  },
  computed: {
    workoutArray: function () {
      return this.products.filter((item) => item.category === '重訓')
    },
    aerobicArray: function () {
      return this.products.filter((item) => item.category === '有氧')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.Lecture:hover{
  cursor:pointer;
  box-shadow:3px 3px grey;
  transition: all .5s;
}
.list-group-item.active{
  background:rgba(228, 225, 225, 0.5);
  font-weight:600;
}
.lecture_option{
  width:50%;
}
.Recommended_class_frame{
  overflow:hidden;
}
.Recommended_class{
  transform:translateX(0px);
  transition:all .5s;
}
.Recommended_class{
  .card-footer{
    .btn{
      width:60%;
      font-size:10px;
      color:grey;
    }
    .btn:hover{
      color:white;
    }
  }
}
.swiper-container2{
  height:100%;
}
.small-text{
  font-size: 14px;
}
.medium-text{
  font-size: 16px;
}
.goTolecture_btn{
  width: 60%;
  margin:0 auto 0 auto;
}
.special_price_text{
  font-size: 1.5rem;
}
.origin_price_text{
  font-size: 1rem;
}
@media(max-width:768px){
  .Recommended_class{
  .card-footer{
    .btn{
      width:100%;
    }
  }
}
}
@media(max-width:680px){
  .lecture_option{
    width:100%;
  }
  .goTolecture_btn{
    width:100%;
  }
}
@media(max-width:380px){
  .special_price_text{
    font-size:1.2rem;
  }
}
</style>
