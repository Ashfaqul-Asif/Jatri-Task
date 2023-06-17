<template>
	<nav aria-label="Page navigation example">
		<ul class="inline-flex items-center -space-x-px">
			<li @click="onChangePageNumber('previous')">
				<a
					href="#"
					class="block px-3 py-2 ml-0 leading-tight text-gray-500 bg-white border border-gray-300 rounded-l-lg"
					:class="
            activePageNumber == 0
              ? 'focus:outline-none opacity-50 pointer-events-none'
              : 'hover:bg-gray-100 hover:text-gray-700'
          "
				>
					<span class="sr-only">Previous</span>
					<svg
						aria-hidden="true"
						class="w-5 h-5"
						fill="currentColor"
						viewBox="0 0 20 20"
						xmlns="http://www.w3.org/2000/svg"
					>
						<path
							fill-rule="evenodd"
							d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
							clip-rule="evenodd"
						/>
					</svg>
				</a>
			</li>
			<li @click="onChangePageNumber(i)" v-for="(page, i) in paginationPageNumber" :key="i">
				<a
					href="#"
					class="px-3 py-2 leading-tight border border-gray-300 hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
					:class="
            activePageNumber == i ? 'text-white bg-blue-700' : 'text-gray-500 bg-white'
          "
				>{{ page }}</a>
			</li>

			<li @click="onChangePageNumber('next')">
				<a
					href="#"
					class="block px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 rounded-r-lg hover:bg-gray-100 hover:text-gray-700"
					:class="
            activePageNumber == paginationPageNumber - 1
              ? 'focus:outline-none opacity-50 pointer-events-none'
              : 'hover:bg-gray-100 hover:text-gray-700'
          "
				>
					<span class="sr-only">Next</span>
					<svg
						aria-hidden="true"
						class="w-5 h-5"
						fill="currentColor"
						viewBox="0 0 20 20"
						xmlns="http://www.w3.org/2000/svg"
					>
						<path
							fill-rule="evenodd"
							d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
							clip-rule="evenodd"
						/>
					</svg>
				</a>
			</li>
		</ul>
	</nav>
</template>

<script setup>
import { computed } from "vue";
const props = defineProps({
	pageSize: {
		type: Number,
		default: null,
	},
	activePageNumber: {
		type: Number,
		default: null,
	},
	totalProducts: {
		type: Number,
		default: null,
	},
});
const emit = defineEmits(["onChangePage"]);
const paginationPageNumber = computed(() => {
	return Math.ceil(props.totalProducts / props.pageSize);
});
const onChangePageNumber = (type) => {
	let pageNumber =
		type === "next"
			? (props.activePageNumber += 1) // if
			: type === "previous"
				? (props.activePageNumber -= 1) // else if
				: type;
	console.log(pageNumber, "pageNumber");
	emit("onChangePage", pageNumber);
};
</script>

<style></style>
