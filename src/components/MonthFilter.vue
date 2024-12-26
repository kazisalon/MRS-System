// /components/MonthFilter.vue
<template>
	<div class="month-filter">
		<div class="month-filter-header">
			<button @click="previousMonth">‹</button>
			<span>{{ currentMonthYear }}</span>
			<button @click="nextMonth">›</button>
		</div>
	</div>
</template>

<script>
export default {
	name: "MonthFilter",
	data() {
		return {
			currentDate: new Date(),
		};
	},
	computed: {
		currentMonthYear() {
			return this.currentDate.toLocaleDateString("en-US", {
				month: "long",
				year: "numeric",
			});
		},
	},
	methods: {
		previousMonth() {
			this.currentDate = new Date(
				this.currentDate.setMonth(this.currentDate.getMonth() - 1)
			);
			this.$emit("month-change", this.currentDate);
		},
		nextMonth() {
			this.currentDate = new Date(
				this.currentDate.setMonth(this.currentDate.getMonth() + 1)
			);
			this.$emit("month-change", this.currentDate);
		},
	},
};
</script>

<style scoped>
.month-filter {
	background: #ffffff;
	border-radius: 8px;
	box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	margin-bottom: 20px;
	width: fit-content;
}

.month-filter-header {
	display: flex;
	align-items: center;
	padding: 12px 20px;
	gap: 15px;
	background: linear-gradient(to right, #2c5282, #2b6cb0);
	border-radius: 8px;
	color: white;
}

.month-filter-header button {
	background: none;
	border: none;
	color: white;
	font-size: 20px;
	cursor: pointer;
	padding: 0 10px;
	transition: opacity 0.2s;
}

.month-filter-header button:hover {
	opacity: 0.8;
}

.month-filter-header span {
	font-weight: 500;
	min-width: 150px;
	text-align: center;
}
</style>
