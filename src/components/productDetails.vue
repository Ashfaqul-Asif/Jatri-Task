<template>
	<td colspan="5" class="py-4 px-6">
		<div class="text-center border-b-2 border-dashed mb-3">
			<h1 class="text-lg font-bold">View details</h1>
		</div>
		<ul class="list-none text-lg">
			<img :src="productDetails.thumbnail" alt srcset />
			<li>Name: {{ productDetails.title }}</li>
			<li>Price: {{ productDetails.price }}</li>
			<li>Rating: {{ productDetails.rating }}</li>
			<li>Description: {{ productDetails.description }}</li>

			<!-- ... -->
		</ul>
	</td>
</template>

<script setup>
const props = defineProps({
	productDetails: {
		type: Object,
		default: null,
	},
});
</script>
