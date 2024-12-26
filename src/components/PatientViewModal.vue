// /components/PatientViewModal.vue (renamed from ViewModal.vue)
<template>
	<div
		class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
	>
		<div class="bg-gray-800 p-6 rounded-lg w-full max-w-lg">
			<h2 class="text-xl mb-4">Patient Details</h2>
			<div v-for="(value, key) in patient" :key="key" class="mb-2">
				<strong class="capitalize">{{ key }}:</strong> {{ value }}
			</div>
			<button
				@click="$emit('close')"
				class="mt-4 bg-blue-500 px-4 py-2 rounded"
			>
				Close
			</button>
		</div>
	</div>
</template>

<script>
export default {
	name: "PatientViewModal",
	props: {
		patient: {
			type: Object,
			required: true,
		},
	},
};
</script>
