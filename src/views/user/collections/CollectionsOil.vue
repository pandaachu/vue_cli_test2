<template>
  <div class="l-background l-collection">
    <Loading :active.sync="isLoading"></Loading>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-10 mt-5">
          <img src="@/../public/images/collectionOil-banner.jpg" alt="">
          <div class="d-flex justify-content-end">
            <div class="l-collection__content p-5" style="background-color:rgba(255,255,255, 0.1); max-width:500px; margin-top:-20% ; margin-right:-6rem">
              <h5 class="l-collection__text text-secondary font-weight-light letter-spacing-xl">單方精油</h5>
              <p class="l-collection__text text-secondary font-weight-lighter letter-spacing-l">精油通常是用低溫蒸餾的方法萃取的。將蒸氣加壓通過植物，就會把精油釋入蒸氣中。等到混合著精油的蒸氣冷卻之後，水和油自然分離，就能取得最純凈的精油了。</p>
              <p class="l-collection__text text-secondary font-weight-lighter letter-spacing-l">為了確保萃取的精油具有正確的化學結構組合和優質的品質，萃取過程中所用的溫度和壓力都必須受到嚴密的控管。溫度和壓力若是不足，將無法讓珍貴的精油釋出，若是太高，則會改變萃取物的構造和效力。</p>
            </div>
          </div>
          <hr class="margin-bottom:5rem">
          <div class="carousel-wrapper border border-secondary border-right-0 border-left-0 my-5">
            <swiper class="swiper-container mt-2" ref="mySwiper" :options="swiperOptions" @mouseenter.native="onSwiperEnter"  @mouseleave.native="onSwiperLeave">
              <swiper-slide  v-for="product in collectionFilter" :key="product.id">
                <router-link :to="`/product/${product.id}`" class="d-flex flex-column align-items-center hoverGroup position-relative my-3">
                  <img
                    :src="product.imageUrl[0]"
                    class="hoverGroup__image rounded-circle border border-secondary"
                    style="width: 150px; height: 150px; object-fit: cover;"
                  >
                  <h6 class="hoverGroup__underline text-secondary pb-2 font-m mt-2">{{ product.title }}</h6>
                  <p class="hoverGroup__showText text-secondary position-absolute font-m m-0" style="top:75px ;transform: translateY(-50%);">看產品</p>
                </router-link>
              </swiper-slide>
            </swiper >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* global $ */
import { gsap } from 'gsap'
export default {
  name: 'CollectionOil',
  data () {
    return {
      swiperOptions: {
        slidesPerView: 5,
        spaceBetween: 30,
        speed: 1500,
        loop: true,
        // centeredSlides: true,
        autoplay: {
          delay: 3000,
          disableOnInteraction: false
        }
      },
      products: [],
      isLoading: false,
      messages: [
        {
          name: '失敗',
          content: '出現錯誤'
        }
      ]
    }
  },
  methods: {
    getProducts () {
      this.isLoading = true
      this.$http.get(`${process.env.VUE_APP_APIPATH}/${process.env.VUE_APP_UUID}/ec/products`)
        .then((res) => {
          this.products = res.data.data
          this.isLoading = false
        })
        .catch(() => {
          this.$bus.$emit('push-messages', this.messages[0])
          $('.l-toast').toast('show')
          this.isLoading = false
        })
    },
    // mouse enter autoplay stop
    onSwiperEnter () {
      this.$refs.mySwiper.$swiper.autoplay.stop()
    },
    // mouse leave autoplay start
    onSwiperLeave () {
      this.$refs.mySwiper.$swiper.autoplay.start()
    }
  },
  created () {
    this.getProducts()
  },
  mounted () {
    gsap.from('.l-collection__content', {
      css: {
        backgroundColor: 'transparent'
      },
      duration: 1.5,
      ease: 'none'
    })
    gsap.from('.l-collection__text', {
      opacity: 0,
      y: 30,
      delay: 0.5,
      duration: 2,
      stagger: 0.8,
      ease: 'power3'
    })
  },
  computed: {
    collectionFilter () {
      const oilCollection = this.products.filter(item => {
        return item.category === '香精油'
      })
      return oilCollection
    },
    swiper () {
      return this.$refs.mySwiper.$swiper
    }
  }
}
</script>
