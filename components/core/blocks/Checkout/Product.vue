<template>
  <div class="flex flex-wrap checkout-product-item  pb-3 mb-3 border-b">
    <div class="w-1/4 px-2">
      <div class="blend">
        <product-image :image="image" />
      </div>
    </div>
    <div class="w-3/4">
      <div class="w-full flex flex-wrap">
        <div class="w-3/4">
          <div class="font-medium cl-accent serif">
            {{ product.name | htmlDecode }}
          </div>
          <div class="text-grey-dark text-sm mt-1">
            {{ product.sku }}
          </div>
          <div class="text-sm mt-2 options" v-if="product.totals && product.totals.options">
            <div v-for="opt in product.totals.options" :key="opt.label">
              <span class="opn text-grey">{{ opt.label }}: </span>
              <span class="opv text-grey-dark" v-html="opt.value" />
            </div>
          </div>
          <div class="text-sm mt-2 options" v-else-if="product.options">
            <div v-for="opt in product.options" :key="opt.label">
              <span class="opn text-grey">{{ opt.label }}: </span>
              <span class="opv text-grey-dark" v-html="opt.value" />
            </div>
          </div>
          <div>
            <span class="text-sm">
              <span class="text-grey">{{ $t('Qty') }}:</span>
              <span class="weight-700 text-grey-dark">
                {{ product.qty }}
              </span>
            </span>
          </div>
        </div>
        <div class="w-1/4 text-right">
          <div v-if="isOnline && product.totals">
            <span class="text-error font-medium block" v-if="product.totals.discount_amount">{{ product.totals.row_total - product.totals.discount_amount + product.totals.tax_amount | price(storeView) }} </span>
            <span class="price-original block text-sm text-grey-dark mt-1" v-if="product.totals.discount_amount" >{{ product.totals.row_total_incl_tax | price(storeView) }}</span>
            <span v-if="!product.totals.discount_amount" class="h4">{{ product.totals.row_total_incl_tax | price(storeView) }}</span>
          </div>
          <div v-else>
            <span class="text-error block font-medium" v-if="product.special_price">{{ product.price_incl_tax * product.qty | price(storeView) }} </span>
            <span class="price-original block text-sm text-grey-dark mt-1" v-if="product.special_price" >{{ product.original_price_incl_tax * product.qty | price(storeView) }}</span>
            <span v-if="!product.special_price" class="h4">{{ product.price_incl_tax * product.qty | price(storeView) }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Product } from '@vue-storefront/core/modules/checkout/components/Product'
import { onlineHelper } from '@vue-storefront/core/helpers'
import ProductImage from 'theme/components/core/ProductImage'
import { currentStoreView } from '@vue-storefront/core/lib/multistore'

export default {
  computed: {
    storeView () {
      return currentStoreView()
    },
    isOnline () {
      return onlineHelper.isOnline
    },
    image () {
      return {
        loading: this.thumbnail,
        src: this.thumbnail
      }
    }
  },
  mixins: [Product],
  components: {
    ProductImage
  }
}
</script>

<style scoped>
.price-original {
  text-decoration: line-through;
}
.blend {
  mix-blend-mode: multiply;
}
</style>
