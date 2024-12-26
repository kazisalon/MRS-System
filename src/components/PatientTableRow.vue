// /components/PatientTableRow.vue (renamed from TableRow.vue)
<template>
	<tr class="border-b bg-gray-800 border-gray-700 hover:bg-gray-600">
		<td v-for="(value, key) in patientData" :key="key" class="px-6 py-4">
			<span v-if="key === 'status'" :class="getStatusClass(value)">
				{{ value }}
			</span>
			<span v-else>{{ value }}</span>
		</td>
		<td class="px-6 py-4">
			<PatientActions
				@view="$emit('view', patient)"
				@edit="$emit('edit', patient)"
				@delete="$emit('delete', patient)"
			/>
		</td>
	</tr>
</template>

<script>
import PatientActions from "./PatientActions.vue";

export default {
	name: "PatientTableRow",
	components: { PatientActions },
	props: {
		patient: {
			type: Object,
			required: true,
		},
	},
	computed: {
		patientData() {
			const {
				id,
				fullName,
				gender,
				age,
				visitDate,
				serviceType,
				diagnosis,
				status,
			} = this.patient;
			return {
				id,
				fullName,
				gender,
				age,
				visitDate,
				serviceType,
				diagnosis,
				status,
			};
		},
	},
	methods: {
		getStatusClass(status) {
			return status === "Complete"
				? "px-2 py-1 rounded-full bg-green-900 text-green-300"
				: "px-2 py-1 rounded-full bg-yellow-900 text-yellow-300";
		},
	},
};
</script>
