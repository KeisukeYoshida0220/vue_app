<template>
	<employee-form-pane :errors="errors" :employee="employee" @submit="createEmployee"></employee-form-pane>
</template>

<script>
import axios from 'axios';
import EmployeeFormPane from 'EmployeeFormPane.vue'

export default {
	components: {
		EmployeeFormPane
	},
	data: function () {
		return {
			employee: {
				name: '',
				department: '',
				gender: '',
				birth: '',
				joined_date: '',
				payment: '',
				note: ''
			},
			errors: ''
		}
	},
	methods: {
		createEmployee: function() {
			axios
			.post('api/v1/employees', this.employee)
			.then( response => {
				let e = response.data;
				this.$router.push({ name: 'EmployeeDetailPage', params: { id: e.id } });
				// template で遷移先を定義する際は <router-link :to="..."> だったけど、プログラム的に行う場合は router.push(location, onComplete?, onAbort?) を使う。これにより router の history スタックに新しいエントリが追加される
			})
			.catch(error => {
				console.log(error);
				if (error.response.data && error.response.data.errors) {
					this.errors = error.response.data.errors;
				}
			});
		}
	}
}
</script>

<style scoped>
	
</style>