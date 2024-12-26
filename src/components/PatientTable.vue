// /components/PatientTable.vue
<template>
	<div class="table-container">
		<div class="table-header">
			<h2>Patient Records</h2>
			<MonthFilter @month-change="filterByMonth" />
		</div>

		<div class="table-responsive">
			<table>
				<thead>
					<tr>
						<th>Patient ID</th>
						<th>Full Name</th>
						<th>Gender</th>
						<th>Age</th>
						<th>Visit Date</th>
						<th>Service Type</th>
						<th>Diagnosis</th>
						<th>Status</th>
						<th>Actions</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="patient in filteredPatients" :key="patient.id">
						<td>{{ patient.id }}</td>
						<td>{{ patient.fullName }}</td>
						<td>{{ patient.gender }}</td>
						<td>{{ patient.age }}</td>
						<td>{{ formatDate(patient.visitDate) }}</td>
						<td>{{ patient.serviceType }}</td>
						<td>{{ patient.diagnosis }}</td>
						<td>
							<span :class="['status-badge', patient.status.toLowerCase()]">
								{{ patient.status }}
							</span>
						</td>
						<td class="actions">
							<button class="btn-view" @click="viewPatient(patient)">
								View
							</button>
							<button class="btn-edit" @click="editPatient(patient)">
								Edit
							</button>
							<button class="btn-delete" @click="deletePatient(patient)">
								Delete
							</button>
						</td>
					</tr>
				</tbody>
			</table>
		</div>

		<!-- Modals -->
		<div v-if="showViewModal" class="modal">
			<div class="modal-content">
				<h3>Patient Details</h3>
				<div class="patient-details">
					<div
						v-for="(value, key) in selectedPatient"
						:key="key"
						class="detail-item"
					>
						<strong>{{ formatLabel(key) }}:</strong> {{ value }}
					</div>
				</div>
				<button class="btn-primary" @click="showViewModal = false">
					Close
				</button>
			</div>
		</div>

		<div v-if="showEditModal" class="modal">
			<div class="modal-content">
				<h3>Edit Patient</h3>
				<form @submit.prevent="savePatient">
					<div class="form-group">
						<label>Full Name</label>
						<input v-model="editingPatient.fullName" type="text" required />
					</div>
					<div class="form-group">
						<label>Age</label>
						<input v-model="editingPatient.age" type="number" required />
					</div>
					<div class="form-group">
						<label>Service Type</label>
						<select v-model="editingPatient.serviceType">
							<option value="OPD">OPD</option>
							<option value="Maternity">Maternity</option>
							<option value="Emergency">Emergency</option>
						</select>
					</div>
					<div class="form-group">
						<label>Diagnosis</label>
						<input v-model="editingPatient.diagnosis" type="text" />
					</div>
					<div class="modal-actions">
						<button type="submit" class="btn-primary">Save</button>
						<button
							type="button"
							class="btn-secondary"
							@click="showEditModal = false"
						>
							Cancel
						</button>
					</div>
				</form>
			</div>
		</div>
	</div>
</template>

<script>
import MonthFilter from "./MonthFilter.vue";

export default {
	name: "PatientTable",
	components: {
		MonthFilter,
	},
	data() {
		return {
			patients: [
				{
					id: "P-001",
					fullName: "Bishal Bashyal",
					gender: "Male",
					age: 45,
					visitDate: "2024-12-20",
					serviceType: "OPD",
					diagnosis: "Diabetes",
					status: "Complete",
				},
				{
					id: "P-001",
					fullName: "Samrat Ghimire",
					gender: "Male",
					age: 45,
					visitDate: "2024-12-20",
					serviceType: "OPD",
					diagnosis: "Diabetes",
					status: "Complete",
				},
				{
					id: "P-031",
					fullName: "Anil Regmi",
					gender: "Male",
					age: 45,
					visitDate: "2024-12-20",
					serviceType: "OPD",
					diagnosis: "Diabetes",
					status: "In Progress",
				},
				{
					id: "P-031",
					fullName: "Puru Karki",
					gender: "Male",
					age: 45,
					visitDate: "2025-1-20",
					serviceType: "OPD",
					diagnosis: "Diabetes",
					status: "In Progress",
				},
			],
			filteredPatients: [],
			selectedPatient: null,
			editingPatient: null,
			showViewModal: false,
			showEditModal: false,
			currentMonth: new Date(),
		};
	},
	methods: {
		filterByMonth(date) {
			this.currentMonth = date;
			this.filteredPatients = this.patients.filter((patient) => {
				const visitDate = new Date(patient.visitDate);
				return (
					visitDate.getMonth() === date.getMonth() &&
					visitDate.getFullYear() === date.getFullYear()
				);
			});
		},
		formatDate(date) {
			return new Date(date).toLocaleDateString();
		},
		formatLabel(key) {
			return key
				.replace(/([A-Z])/g, " $1")
				.replace(/^./, (str) => str.toUpperCase());
		},
		viewPatient(patient) {
			this.selectedPatient = patient;
			this.showViewModal = true;
		},
		editPatient(patient) {
			this.editingPatient = { ...patient };
			this.showEditModal = true;
		},
		savePatient() {
			const index = this.patients.findIndex(
				(p) => p.id === this.editingPatient.id
			);
			if (index !== -1) {
				this.patients[index] = this.editingPatient;
				this.filterByMonth(this.currentMonth);
			}
			this.showEditModal = false;
		},
		deletePatient(patient) {
			if (confirm("Are you sure you want to delete this patient?")) {
				this.patients = this.patients.filter((p) => p.id !== patient.id);
				this.filterByMonth(this.currentMonth);
			}
		},
	},
	created() {
		this.filteredPatients = [...this.patients];
	},
};
</script>

<style scoped>
.table-container {
	background: #ffffff;
	border-radius: 12px;
	box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
	padding: 20px;
	margin-top: 10%;
}

.table-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 20px;
	padding-bottom: 20px;
	border-bottom: 1px solid #e2e8f0;
}

.table-header h2 {
	color: #2d3748;
	font-size: 1.5rem;
	font-weight: 600;
}

.table-responsive {
	overflow-x: auto;
}

table {
	width: 100%;
	border-collapse: collapse;
	margin-top: 20px;
}

th {
	background: #2b6cb0;
	color: white;
	padding: 12px;
	text-align: left;
	font-weight: 500;
}

td {
	padding: 12px;
	border-bottom: 1px solid #e2e8f0;
	color: #4a5568;
}

tr:hover {
	background-color: #f7fafc;
}

.status-badge {
	padding: 4px 8px;
	border-radius: 12px;
	font-size: 0.875rem;
	font-weight: 500;
}

.status-badge.complete {
	background-color: #c6f6d5;
	color: #22543d;
}

.status-badge.incomplete {
	background-color: #fed7d7;
	color: #742a2a;
}

.actions {
	display: flex;
	gap: 8px;
}

.btn-view,
.btn-edit,
.btn-delete {
	padding: 6px 12px;
	border-radius: 6px;
	border: none;
	cursor: pointer;
	font-size: 0.875rem;
	transition: opacity 0.2s;
}

.btn-view {
	background: #4299e1;
	color: white;
}

.btn-edit {
	background: #48bb78;
	color: white;
}

.btn-delete {
	background: #f56565;
	color: white;
}

.btn-view:hover,
.btn-edit:hover,
.btn-delete:hover {
	opacity: 0.9;
}

/* Modal Styles */
.modal {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(0, 0, 0, 0.5);
	display: flex;
	align-items: center;
	justify-content: center;
	z-index: 1000;
}

.modal-content {
	background: white;
	padding: 24px;
	border-radius: 12px;
	width: 90%;
	max-width: 500px;
	max-height: 90vh;
	overflow-y: auto;
}

.modal-content h3 {
	color: #2d3748;
	font-size: 1.25rem;
	font-weight: 600;
	margin-bottom: 20px;
}

.patient-details {
	margin-bottom: 20px;
}

.detail-item {
	margin-bottom: 12px;
}

.form-group {
	margin-bottom: 16px;
}

.form-group label {
	display: block;
	margin-bottom: 8px;
	color: #4a5568;
	font-weight: 500;
}

.form-group input,
.form-group select {
	width: 100%;
	padding: 8px;
	border: 1px solid #e2e8f0;
	border-radius: 6px;
	font-size: 1rem;
}

.modal-actions {
	display: flex;
	justify-content: flex-end;
	gap: 12px;
	margin-top: 24px;
}

.btn-primary,
.btn-secondary {
	padding: 8px 16px;
	border-radius: 6px;
	border: none;
	cursor: pointer;
	font-weight: 500;
	transition: opacity 0.2s;
}

.btn-primary {
	background: #4299e1;
	color: white;
}

.btn-secondary {
	background: #718096;
	color: white;
}

.btn-primary:hover,
.btn-secondary:hover {
	opacity: 0.9;
}
</style>
