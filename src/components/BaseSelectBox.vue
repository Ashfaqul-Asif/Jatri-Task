<template>
	<div class="mx-2">
		<label class="block mb-2 text-sm font-medium text-gray-900">{{ label }}:</label>

		<select
			class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg"
			:value="modelValue"
			@input="updateValue"
		>
			<option v-for="option in options" :key="option.value" :value="option.value">{{ option.label }}</option>
		</select>
	</div>
</template>

<script setup>
import { ref, watch } from "vue";
const emit = defineEmits(["update:modelValue"]);
const props = defineProps({
	label: {
		type: String,
		required: true,
	},
	options: {
		type: Array,
		required: true,
	},
	modelValue: {
		type: [String, Number],
		required: true,
	},
});
const updateValue = (event) => {
	emit("update:modelValue", event.target.value);
};
</script>
