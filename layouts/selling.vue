<template lang="pug">
  .b-template
    header-component
    transition(
      name="fade"
      mode="out-in"
    )
      Nuxt
    reviews-component
    products-component(
      title="Продать другие напитки"
    )
    questions-component
    steps-component
    popularity-component
    footer-component
</template>

<script>
import { Component, mixins } from 'nuxt-property-decorator'
import Reviews from '@/components/sections/reviews'
import Products from '@/components/sections/products'
import Questions from '@/components/sections/questions'
import Steps from '@/components/sections/steps'
import Popularity from '@/components/sections/popularity'
import Header from '@/components/blanks/header'
import Footer from '@/components/blanks/footer'
import AosMixin from '@/mixins/aos'
import getOs from '@/ts/getOs'

@Component({
  components: {
    'popularity-component': Popularity,
    'steps-component': Steps,
    'questions-component': Questions,
    'products-component': Products,
    'reviews-component': Reviews,
    'header-component': Header,
    'footer-component': Footer
  }
})
export default class SellingLayout extends mixins(AosMixin) {
  mounted () {
    document.documentElement.classList.add(`os-${getOs()}`)

    // @ts-ignore
    this.$scrollTo('body')
  }
}
</script>
