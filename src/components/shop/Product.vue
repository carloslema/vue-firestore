<template>
	<v-card>
		<v-card-media
		  	contain
		  	height="300px"
		  	:src="product.url"
		/>

		<v-card-actions>
			<v-spacer />
			<span>
				<v-chip label color="pink" text-color="white">
					<v-icon>label</v-icon> {{product.name}}
				</v-chip>
				<v-chip label outline color="red">
					{{product.price}}
				</v-chip>
			</span>

			<v-btn icon @click="$emit('addToCart')" v-if="$store.getters.isCustomer">
				<v-icon>add_shopping_cart</v-icon>
			</v-btn>
		</v-card-actions>
			
	</v-card>
</template>

<script>
export default {

  	name: 'Product',
  	props: {
  		product: {
  			type: Object,
  			required: true
  		}
  	},
  	data () {
	    return {

	    }
  	}
}
</script>

<style lang="css" scoped>
</style>