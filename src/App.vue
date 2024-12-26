// App.vue
<template>
	<div class="min-h-screen bg-gray-900 text-white p-8">
		<PatientTable />
		<PatientHeader />
	</div>
</template>

<script>
import PatientTable from "./components/PatientTable.vue";
import PatientHeader from "./components/PatientHeader.vue";
export default {
	name: "App",
	components: {
		PatientTable,
		PatientHeader,
	},
};
</script>
