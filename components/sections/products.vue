<template lang="pug">
  section.b-products
    .container.__container
      h2.__title(
        v-if="title"
        :data-aos-delay="100"
        data-aos="fade-right"
      ) {{title}}
      .__wrapper
        NuxtLink.__card(
          v-for="product in products"
          :key="product.id"
          :to="product.to"
          v-on:click.native="goToTop"
        )
          card-product-component(
            :image="product.image"
            :name="product.name"
            :data-aos-delay="150+(product.id*100)"
            data-aos="fade"
            data-aos-offset="0"
          )
</template>

<script lang="ts">
import { Component, mixins, Prop } from 'nuxt-property-decorator'
import CardProduct from '~/components/blanks/card-product.vue'
import AosMixin from '~/mixins/aos'

@Component({
  components: {
    'card-product-component': CardProduct
  }
})
export default class Products extends mixins(AosMixin) {
  @Prop() readonly title!: string

  products: {
    id: number
    name: string
    image: string
    to: string
  }[] = []

  // products = [
  //   {
  //     id: 1,
  //     imageName: 'vine',
  //     name: 'Вино',
  //     to: '/selling/вино'
  //   },
  //   {
  //     id: 2,
  //     imageName: 'kon',
  //     name: 'Коньяк',
  //     to: '/selling/коньяк'
  //   },
  //   {
  //     id: 3,
  //     imageName: 'viski',
  //     name: 'Виски',
  //     to: '/selling/виски'
  //   },
  //   {
  //     id: 4,
  //     imageName: 'vodka',
  //     name: 'Водка',
  //     to: '/selling/водка'
  //   },
  //   {
  //     id: 5,
  //     imageName: 'shamp',
  //     name: 'Шампанское',
  //     to: '/selling/шампанское'
  //   }
  // ]

  async fetch (this: Products) {
    return await this.$axios.get('/alcohol/categories')
      .then((response: {
        data: {
          id: number
          author: string
          image: string
          name: string
        }[]
      }) => {
        this.products = response.data.map((product) => {
          return {
            id: product.id,
            name: product.name,
            image: product.image,
            to: `/selling/${product.name}`
          }
        })
      })
      .catch((error: any) => console.log(error))
  }

  goToTop () {
    // @ts-ignore
    this.$scrollTo('body')
  }
}
</script>
