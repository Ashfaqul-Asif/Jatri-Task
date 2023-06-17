<template>
	<div class="container py-5">
		<div class="flex justify-end">
			<BaseSelectBox v-model="priceSort" label="Price" :options="sortingDropDown" />
			<BaseSelectBox v-model="ratingSort" label="Rating" :options="sortingDropDown" />
		</div>
		<div class="relative border border-gray-200 rounded-lg shadow-sm my-6">
			<table class="w-full text-sm text-left text-gray-500">
				<thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700">
					<tr>
						<th scope="col" class="px-6 py-3">SI</th>
						<th scope="col" class="px-6 py-3">Name</th>
						<th scope="col" class="px-6 py-3">Rating</th>
						<th scope="col" class="px-6 py-3">Price</th>
						<th scope="col" class="px-6 py-3">Action</th>
					</tr>
				</thead>
				<tbody>
					<template v-for="(product, index) in finalProductList">
						<tr class="bg-white border-b hover:bg-gray-50" :key="index" v-if="index >= 0">
							<th
								scope="row"
								class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"
							>{{ product.id }}</th>
							<td class="px-6 py-4">{{ product.title }}</td>
							<td class="px-6 py-4">{{ product.rating }}</td>
							<td class="px-6 py-4">{{ product.price }}</td>
							<td class="px-6 py-4">
								<button
									v-if="productDetailsId == product.id"
									class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
									@click="closeDetails"
								>Close</button>
								<button
									v-else
									class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
									@click="getProductsDetails(product.id)"
								>Show</button>
							</td>
						</tr>
						<tr v-if="productDetailsId == product.id" :key="index + '1'">
							<product-details v-if="productDetailsData" :productDetails="productDetailsData" />
						</tr>
					</template>
				</tbody>
			</table>
		</div>
		<pagination
			@onChangePage="onChangePage"
			:pageSize="perPage"
			:activePageNumber="activePage"
			:totalProducts="allProducts.total"
		/>
	</div>
</template>

<script setup>
import productDetails from "../components/productDetails.vue";
import BaseSelectBox from "../components/BaseSelectBox.vue";
import pagination from "../components/pagination.vue";
import axios from "axios";
import { ref, onMounted, computed } from "vue";

const allProducts = ref({ products: [] });
const productDetailsData = ref(null);
const productDetailsId = ref(null);
const ratingSort = ref("des");
const priceSort = ref("asc");

const perPage = ref(5);
const activePage = ref(0);
const getProducts = async () => {
	let response = await axios.get("https://dummyjson.com/products?limit=0");
	allProducts.value = response.data;
};
const sortingDropDown = ref([
	{
		label: "Ascending",
		value: "asc",
	},
	{
		label: "Descending",
		value: "des",
	},
]);
const getProductsDetails = async (id) => {
	productDetailsId.value = id;
	let response = await axios.get(`https://dummyjson.com/products/${id}`);
	productDetailsData.value = response.data;
};
const closeDetails = () => {
	productDetailsId.value = null;
	productDetailsData.value = {};
};
const finalProductList = computed(() => {
	let startItem = activePage.value * perPage.value;
	let endItem = startItem + perPage.value;
	let sortedProducts = [...allProducts.value.products];
	if (priceSort.value) {
		sortedProducts.sort((a, b) => {
			return priceSort.value === "asc" ? a.price - b.price : b.price - a.price;
		});
	}
	if (ratingSort.value) {
		sortedProducts.sort((a, b) => {
			return ratingSort.value === "asc" ? a.rating - b.rating : b.rating - a.rating;
		});
	}
	return sortedProducts.length ? sortedProducts.slice(startItem, endItem) : [];
	// return conditions.find((a) => a);
});

const onChangePage = (pageNumber) => {
	activePage.value = pageNumber;
};
onMounted(() => {
	getProducts();
});
</script>
