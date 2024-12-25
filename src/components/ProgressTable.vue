<template>
	<div class="container mx-auto p-4">
		<table class="w-full border-collapse border">
			<thead>
				<tr>
					<th colspan="5" class="border p-2 text-center">
						{{ currentMonth }}/{{ currentYear }}
					</th>
				</tr>
				<tr>
					<th class="border p-2">Name</th>
					<th class="border p-2">Address</th>
					<th class="border p-2">Class</th>
					<th class="border p-2">In Progress</th>
					<th class="border p-2">Completed</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="student in students" :key="student.id">
					<td class="border p-2">{{ student.name }}</td>
					<td class="border p-2">{{ student.address }}</td>
					<td class="border p-2">{{ student.class }}</td>
					<td class="border p-2 text-center">
						<button
							class="bg-black text-white px-4 py-1 rounded"
							@click="toggleProgress(student.id)"
						>
							Button
						</button>
					</td>
					<td class="border p-2 text-center">
						<button
							class="bg-black text-white px-4 py-1 rounded"
							@click="toggleCompleted(student.id)"
						>
							Button
						</button>
					</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
import axios from "axios";

export default {
	name: "StudentProgressTable",
	data() {
		return {
			students: [],
			currentMonth: new Date().toLocaleString("default", { month: "long" }),
			currentYear: new Date().getFullYear(),
		};
	},
	methods: {
		async fetchStudents() {
			try {
				const response = await axios.get("YOUR_API_ENDPOINT");
				this.students = response.data;
			} catch (error) {
				console.error("Error fetching students:", error);
			}
		},
		async toggleProgress(studentId) {
			try {
				await axios.post(`YOUR_API_ENDPOINT/progress/${studentId}`);
				await this.fetchStudents();
			} catch (error) {
				console.error("Error updating progress:", error);
			}
		},
		async toggleCompleted(studentId) {
			try {
				await axios.post(`YOUR_API_ENDPOINT/completed/${studentId}`);
				await this.fetchStudents();
			} catch (error) {
				console.error("Error updating completion:", error);
			}
		},
	},
	mounted() {
		this.fetchStudents();
	},
};
</script>
