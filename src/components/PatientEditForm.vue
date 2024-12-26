// /components/PatientEditForm.vue (renamed from EditForm.vue)
<template>
	<div
		class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
	>
		<form
			@submit.prevent="handleSubmit"
			class="bg-gray-800 p-6 rounded-lg w-full max-w-lg"
		>
			<h2 class="text-xl mb-4">Edit Patient</h2>
			<div class="space-y-4">
				<div v-for="(value, key) in formData" :key="key">
					<label class="block mb-1 capitalize">{{ key }}</label>
					<input
						v-model="formData[key]"
						:type="getInputType(key)"
						class="w-full bg-gray-700 rounded px-3 py-2"
					/>
				</div>
			</div>
			<div class="mt-6 flex gap-4">
				<button type="submit" class="bg-green-500 px-4 py-2 rounded">
					Save
				</button>
				<button @click="$emit('close')" class="bg-gray-600 px-4 py-2 rounded">
					Cancel
				</button>
			</div>
		</form>
	</div>
</template>

<script>
export default {
	name: "PatientEditForm",
	props: {
		patient: {
			type: Object,
			required: true,
		},
	},
	data() {
		return {
			formData: { ...this.patient },
		};
	},
	methods: {
		getInputType(key) {
			const types = {
				age: "number",
				visitDate: "date",
			};
			return types[key] || "text";
		},
		handleSubmit() {
			this.$emit("save", this.formData);
			this.$emit("close");
		},
	},
};
</script>
