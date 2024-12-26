// /components/PatientActions.vue (renamed from Actions.vue)
<template>
	<div class="flex gap-2">
		<button @click="$emit('view')" class="text-blue-500 hover:text-blue-400">
			View
		</button>
		<button @click="$emit('edit')" class="text-green-500 hover:text-green-400">
			Edit
		</button>
		<button @click="$emit('delete')" class="text-red-500 hover:text-red-400">
			Delete
		</button>
	</div>
</template>

<script>
export default {
	name: "PatientActions",
};
</script>
