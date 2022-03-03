<template>

<tbody v-if="productsFromCart">
	<tr
      v-for="product in productsFromCart"
      :key="product.productId"
      class="woocommerce-cart-form__cart-item cart_item"  
    >
      				
<template>
<td class="product-remove">
	<CloseOrDeleteButton
			  :class="$style.remove"
			  button-type="delete"
			  @click.native="onRemoveClickHandler(product)"
			/>
</td>

<td class="product-thumbnail">
<nuxt-link :to="`/product/${product.meta.pSlug}`">
          <img
            v-lazy="product.meta.images.imgL"
            :class="$style.image"
          />
        </nuxt-link>
</td>

<td class="product-name" data-title="Товар">
	<nuxt-link :class="$style.pName" :to="`/product/${product.meta.pSlug}`">
          <p>{{ product.meta.pName }}</p>
	</nuxt-link>
</td>

<td class="product-price" data-title="Цена">
<span class="woocommerce-Price-amount amount"><bdi>{{ product.meta.pPrice }}<span class="woocommerce-Price-currencySymbol">грн</span></bdi></span>								
</td>

<td class="product-quantity" data-title="Количество">
<div class="quantity">
<input type="button" value="-" class="minus">
<input
	:value="product.qty"
	:class="$style.input"
	type="number"
	:min="1"
	:max="1000"
	:step="1"
	@input.prevent="onQuantityChangeHandler($event, product)"
/>
<input type="button" value="+" class="plus">
</div>
</td>

<td class="product-subtotal" data-title="Сумма">
<span class="woocommerce-Price-amount amount"><bdi>{{ (product.meta.pPrice * product.qty) | round }}<span class="woocommerce-Price-currencySymbol">грн</span></bdi></span>								</td>
</template>
</tr>


  </tbody>
</template>
<!--  <tbody >
    <tr
      v-for="product in productsFromCart"
      :key="product.productId"
      :class="$style.product"  
    >
      <template>
      <td>
        <nuxt-link :to="`/product/${product.meta.pSlug}`">
          <img
            v-lazy="product.meta.images.imgL"
            :class="$style.image"
          />
        </nuxt-link>
		</td>
		<td class="cart-title">
        <nuxt-link :class="$style.pName" :to="`/product/${product.meta.pSlug}`">
          <p>{{ product.meta.pName }}</p>
        </nuxt-link>
		</td>
		<td>
			<div>
			  <p>{{ product.meta.pPrice }}</p>
			</div>
		</td>
		<td>
			<div>
			  <input
				:value="product.qty"
				:class="$style.input"
				type="number"
				:min="1"
				:max="1000"
				@input.prevent="onQuantityChangeHandler($event, product)"
			  />
			</div>
		</td>
		<td>
			<div>
			  <p>{{ (product.meta.pPrice * product.qty) | round }}</p>
			</div>
		</td>
		<td>
			
		</td>
      </template>
    </tr>
  </tbody>
</template>-->

<script>
import CloseOrDeleteButton from '~~/components/common/input/CloseOrDeleteButton.vue'
import round from '~~/mixins/round'
import { mapActions } from 'vuex'
import debounce from 'lodash.debounce'
export default {
  components: {
    CloseOrDeleteButton
  },
  mixins: [round],
  props: {
    productsFromCart: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    ...mapActions({
      setProductQuantity: 'cart/setProductQuantity',
      removeProduct: 'cart/removeProduct'
    }),
    onRemoveClickHandler (product) {
      this.removeProduct(product.productId)
    },
    onQuantityChangeHandler: debounce(function onQuantityChangeHandler (e, product) {
      const qty = e.target.value
      this.setProductQuantity({ productId: product.productId, qty })
    }, 400)

  }
}
</script>

<style lang="scss" module>
.input {
  height: 20px;
}
    .remove {
      top: -15px;
      left: -30px;
      z-index: 1;
    }
.wrapper {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  .product {
    position: relative;
    margin: 1em;
    display: flex;
    flex-direction: row;

    * {
      margin-right: 10px;
    }
    .pName {
      width: 150px;
    }
  }

  p {
    max-width: 270px;
    height: 35px;
  }
}
.image {
  width: 75px;
  height: 75px;
  object-fit: cover;
}

table{
	margin-left: auto;
	margin-right: auto;
}

  .cart-list-item{
    border-bottom: 1px solid #ddd;
    border-top: 1px solid #ddd;
    padding: 1rem 0;
    
      font-weight: 500;
	  }
    .cart-list> td{
      padding: 1rem 0.5rem;
}
</style>
