<template>
  <div :class="basketClass">
    <template v-if="getBasket.length && !orderPlaced">
        <p class="popup-basket__name">В корзине:</p>
        <div class="popup-basket__list">
          <popup-basket-product class="popup-basket__item" v-for="product in getBasket" :product="product" :key="product.id" />
        </div>
        <popup-basket-form
          @newOrder="newOrder"
        />
    </template>
    <v-notification v-else-if="!getBasket.length && !orderPlaced" text="В корзине ничего нет" vImage="cart" />
    <v-notification v-else vImage="arrow"text="Заказ успешно создан" />
  </div>
</template>
<script>
// vuex
import {mapActions, mapGetters} from "vuex";

// components
import popupBasketProduct from "./popup-basket-product/popup-basket-product";
import popupBasketForm from './popup-basket-form/popup-basket-form'
import vNotification from '@/components/ui-kit/v-notification/v-notification'

export default {
  name: "popup-basket",
  data: ()=> ({
    orderPlaced: false,
  }),
  methods: {
    ...mapActions(['clearBasket']),
    newOrder() {
      this.orderPlaced = true
      this.clearBasket()
    }
  },
  computed: {
    ...mapGetters(['getBasket']),
    basketClass() {
      if(this.orderPlaced) return 'popup-basket--placed'
      return this.getBasket.length && !this.orderPlaced ? 'popup-basket' : 'popup-basket--empty'
    }
  },
  components: {
    popupBasketProduct, popupBasketForm,vNotification
  }
}
</script>
<style lang="scss" src="./popup-basket.scss" scoped />