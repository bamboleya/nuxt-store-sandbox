<template>
	<div class="flex">
		
		<div class="col-checkout">
		  <table class="shop_table shop_table_responsive cart woocommerce-cart-form__contents" cellspacing="0">
			<thead>
					<tr>
						<th class="product-remove">&nbsp;</th>
						<th class="product-thumbnail">&nbsp;</th>
						<th class="product-name">Товар</th>
						<th class="product-price">Цена</th>
						<th class="product-quantity">Количество</th>
						<th class="product-subtotal">Сумма</th>
					</tr>
				</thead>
			  <ProductsList class="products" :products-from-cart="getProducts" />
		  </table>
		  
		</div> 
	</div>
</template>
<script>
import { mapGetters } from 'vuex'
import ProductsList from '~~/components/cart/ProductsList.vue'
import CloseOrDeleteButton from '~~/components/common/input/CloseOrDeleteButton.vue'
import round from '~~/mixins/round.js'
export default {
  components: {
    ProductsList,
    CloseOrDeleteButton
  },
  mixins: [round],
  data () {
    return {
      addedProduct: null,
      defaults: {
        addedProduct: null
      }
    }
  },

  computed: {
    ...mapGetters({
      getProductsInCart: 'cart/getProductsInCart'
    }),
    getAddedProduct () {
      const product = this.getProductsInCart.find(
        prod => prod.productId === this.addedProduct
      )
      if (product) {
        return [product]
      } else {
        return null
      }
    },
    getAmount () {
      let amount = 0
      if (this.getProductsInCart) {
        this.getProductsInCart.forEach(product => {
          amount +=
            parseFloat(product.meta.pPrice) *
            parseInt(product.qty)
        })
        return amount
      } else {
        return 0
      }
    },
    getProducts () {
      if (this.addedProduct) {
        return this.getProductsInCart.filter(
          prod => prod.productId !== this.addedProduct
        )
      } else {
        return this.getProductsInCart
      }
    }

  },

 
  methods: {
    async beforeOpen (event) {
      if (!event.params) {
        event.params = {}
      }
      if (event.params.addedProduct) {
        this.addedProduct = event.params.addedProduct
      } else {
        this.addedProduct = this.defaults.addedProduct
      }
    }
  }
}
</script>
<style lang="scss" scoped>
	@media(max-width: 767px){
	.cart-list{
	width: 100%;
    display: block;
    overflow-x: auto;}
	.col-checkout{width: 100%;}
	}
	
	.col-checkout{width: 50%;}
	.flex{display:block; width: 100%;}
	
  .cart-list-header > th{
      padding: 0.5rem}
</style>
