// /components/PatientDeleteModal.vue (renamed from DeleteConfirmation.vue)
<template>
	<div
		class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
	>
		<div class="bg-gray-800 p-6 rounded-lg w-full max-w-md text-center">
			<h2 class="text-xl mb-4">Confirm Deletion</h2>
			<p class="mb-6">
				Are you sure you want to delete patient {{ patient.fullName }}?
			</p>
			<div class="flex gap-4 justify-center">
				<button
					@click="$emit('confirm', patient.id)"
					class="bg-red-500 px-4 py-2 rounded"
				>
					Delete
				</button>
				<button @click="$emit('close')" class="bg-gray-600 px-4 py-2 rounded">
					Cancel
				</button>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "PatientDeleteModal",
	props: {
		patient: {
			type: Object,
			required: true,
		},
	},
};
</script>
