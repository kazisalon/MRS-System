// /components/PatientTable.vue (renamed from Table.vue)
<template>
	<div class="w-full overflow-x-auto shadow-md rounded-lg">
		<table class="w-full text-sm text-left text-gray-200">
			<thead class="text-xs uppercase bg-gray-700">
				<tr>
					<th v-for="header in headers" :key="header" class="px-6 py-3">
						{{ header }}
					</th>
				</tr>
			</thead>
			<tbody>
				<PatientTableRow
					v-for="patient in patients"
					:key="patient.id"
					:patient="patient"
					@view="openViewModal"
					@edit="openEditForm"
					@delete="openDeleteConfirmation"
				/>
			</tbody>
		</table>
		<PatientViewModal
			v-if="showViewModal"
			:patient="selectedPatient"
			@close="showViewModal = false"
		/>
		<PatientEditForm
			v-if="showEditForm"
			:patient="selectedPatient"
			@close="showEditForm = false"
			@save="handleSave"
		/>
		<PatientDeleteModal
			v-if="showDeleteModal"
			:patient="selectedPatient"
			@close="showDeleteModal = false"
			@confirm="handleDelete"
		/>
	</div>
</template>

<script>
import PatientTableRow from "./PatientTableRow.vue";
import PatientViewModal from "./PatientViewModal.vue";
import PatientEditForm from "./PatientEditForm.vue";
import PatientDeleteModal from "./PatientDeleteModal.vue";

export default {
	name: "PatientTable",
	components: {
		PatientTableRow,
		PatientViewModal,
		PatientEditForm,
		PatientDeleteModal,
	},
	data() {
		return {
			headers: [
				"Patient ID",
				"Full Name",
				"Gender",
				"Age",
				"Visit Date",
				"Service Type",
				"Diagnosis",
				"Status",
				"Actions",
			],
			patients: [
				{
					id: "P-001",
					fullName: "John Doe",
					gender: "Male",
					age: 45,
					visitDate: "2024-12-20",
					serviceType: "OPD",
					diagnosis: "Diabetes",
					status: "Complete",
				},
			],
			selectedPatient: null,
			showViewModal: false,
			showEditForm: false,
			showDeleteModal: false,
		};
	},
	methods: {
		openViewModal(patient) {
			this.selectedPatient = patient;
			this.showViewModal = true;
		},
		openEditForm(patient) {
			this.selectedPatient = patient;
			this.showEditForm = true;
		},
		openDeleteConfirmation(patient) {
			this.selectedPatient = patient;
			this.showDeleteModal = true;
		},
		handleSave(updatedPatient) {
			const index = this.patients.findIndex((p) => p.id === updatedPatient.id);
			if (index !== -1) {
				this.patients[index] = updatedPatient;
			}
			this.showEditForm = false;
		},
		handleDelete(patientId) {
			this.patients = this.patients.filter((p) => p.id !== patientId);
			this.showDeleteModal = false;
		},
	},
};
</script>
