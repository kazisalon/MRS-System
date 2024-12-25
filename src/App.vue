<template>
	<div class="app-container">
		<div class="table-container">
			<div class="date-navigation">
				<button @click="previousMonth" class="nav-button">&lt;</button>
				<h2 class="table-header">{{ currentMonth }}/{{ currentYear }}</h2>
				<button @click="nextMonth" class="nav-button">&gt;</button>
			</div>
			<table>
				<thead>
					<tr>
						<th>Name</th>
						<th>Address</th>
						<th>Class</th>
						<th>In Progress</th>
						<th>Completed</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="student in currentMonthData" :key="student.id">
						<td>{{ student.name }}</td>
						<td>{{ student.address }}</td>
						<td>{{ student.class }}</td>
						<td class="button-cell">
							<button
								class="action-button"
								@click="openModal(student, 'progress')"
							>
								Button
							</button>
						</td>
						<td class="button-cell">
							<button
								class="action-button"
								@click="openModal(student, 'completed')"
							>
								Button
							</button>
						</td>
					</tr>
				</tbody>
			</table>
		</div>

		<!-- Modal -->
		<div v-if="showModal" class="modal-overlay" @click="closeModal">
			<div class="modal-content" @click.stop>
				<h3>Update Status</h3>
				<p>Student: {{ selectedStudent?.name }}</p>
				<p>
					Current Status:
					{{ modalType === "progress" ? "In Progress" : "Completed" }}
				</p>
				<div class="modal-actions">
					<button class="modal-button confirm" @click="confirmStatus">
						Confirm
					</button>
					<button class="modal-button cancel" @click="closeModal">
						Cancel
					</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "StudentTable",
	data() {
		return {
			currentDate: new Date(),
			showModal: false,
			selectedStudent: null,
			modalType: null,
			monthlyData: {
				"2024-12": [
					{
						id: 1,
						name: "John Doe",
						address: "123 Main St",
						class: "Math",
						inProgress: false,
						completed: false,
					},
					{
						id: 2,
						name: "Jane Smith",
						address: "456 Oak Ave",
						class: "Science",
						inProgress: false,
						completed: false,
					},
				],
				"2024-11": [
					{
						id: 3,
						name: "Alice Brown",
						address: "789 Pine Rd",
						class: "History",
						inProgress: false,
						completed: false,
					},
				],
				"2025-01": [
					{
						id: 4,
						name: "Charlie Davis",
						address: "654 Maple Dr",
						class: "Physics",
						inProgress: false,
						completed: false,
					},
				],
			},
		};
	},
	computed: {
		currentMonth() {
			return this.currentDate.toLocaleString("default", { month: "long" });
		},
		currentYear() {
			return this.currentDate.getFullYear();
		},
		currentMonthKey() {
			return `${this.currentYear}-${String(
				this.currentDate.getMonth() + 1
			).padStart(2, "0")}`;
		},
		currentMonthData() {
			return this.monthlyData[this.currentMonthKey] || [];
		},
	},
	methods: {
		previousMonth() {
			this.currentDate = new Date(
				this.currentDate.setMonth(this.currentDate.getMonth() - 1)
			);
		},
		nextMonth() {
			this.currentDate = new Date(
				this.currentDate.setMonth(this.currentDate.getMonth() + 1)
			);
		},
		openModal(student, type) {
			this.selectedStudent = student;
			this.modalType = type;
			this.showModal = true;
		},
		closeModal() {
			this.showModal = false;
			this.selectedStudent = null;
			this.modalType = null;
		},
		confirmStatus() {
			if (this.selectedStudent && this.modalType) {
				if (this.modalType === "progress") {
					this.selectedStudent.inProgress = !this.selectedStudent.inProgress;
				} else {
					this.selectedStudent.completed = !this.selectedStudent.completed;
				}
			}
			this.closeModal();
		},
	},
};
</script>

<style scoped>
.app-container {
	min-height: 100vh;
	background-color: #f5f7fa;
	padding: 20px;
}

.table-container {
	max-width: 1000px;
	margin: 0 auto;
	background: white;
	border-radius: 10px;
	box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
	padding: 20px;
}

.date-navigation {
	display: flex;
	align-items: center;
	justify-content: center;
	gap: 1rem;
	margin-bottom: 2rem;
}

.nav-button {
	padding: 8px 16px;
	font-size: 1.2rem;
	border: none;
	background-color: #f0f2f5;
	border-radius: 6px;
	cursor: pointer;
	transition: background-color 0.2s;
}

.nav-button:hover {
	background-color: #e4e6e9;
}

.table-header {
	font-size: 1.5rem;
	color: #1a1a1a;
	min-width: 200px;
	text-align: center;
}

table {
	width: 100%;
	border-collapse: separate;
	border-spacing: 0;
}

th,
td {
	padding: 15px;
	text-align: left;
	border-bottom: 1px solid #eee;
}

th {
	background-color: #f8f9fa;
	font-weight: 600;
	color: #444;
}

tr:hover {
	background-color: #f8f9fa;
}

.action-button {
	padding: 8px 16px;
	background-color: #000;
	color: white;
	border: none;
	border-radius: 4px;
	cursor: pointer;
	transition: opacity 0.2s;
}

.action-button:hover {
	opacity: 0.9;
}

/* Modal Styles */
.modal-overlay {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background-color: rgba(0, 0, 0, 0.5);
	display: flex;
	justify-content: center;
	align-items: center;
	z-index: 1000;
}

.modal-content {
	background: white;
	padding: 24px;
	border-radius: 8px;
	min-width: 300px;
	max-width: 90%;
	box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.modal-content h3 {
	margin: 0 0 16px 0;
	color: #1a1a1a;
	font-size: 1.25rem;
}

.modal-content p {
	margin: 8px 0;
	color: #444;
}

.modal-actions {
	display: flex;
	gap: 12px;
	margin-top: 24px;
	justify-content: flex-end;
}

.modal-button {
	padding: 8px 16px;
	border: none;
	border-radius: 4px;
	cursor: pointer;
	font-weight: 500;
	transition: opacity 0.2s;
}

.modal-button.confirm {
	background-color: #000;
	color: white;
}

.modal-button.cancel {
	background-color: #e0e0e0;
	color: #333;
}

.modal-button:hover {
	opacity: 0.9;
}

@media (max-width: 768px) {
	.table-container {
		margin: 10px;
		padding: 15px;
	}

	th,
	td {
		padding: 10px;
		font-size: 14px;
	}

	.action-button {
		padding: 6px 12px;
		font-size: 14px;
	}
}
</style>
