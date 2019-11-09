<template>
	<form @submit.prevent="$emit('submit')">
		<!-- @submit には form の submit イベントが発行されたときのイベントハンドラを処理するためのメソッドを設定 -->
		<!-- .prevent と続いていることで、通常の submit で処理されるページリロードを行わないようにする。これは event.preventDefault() と同じ効果 -->
		<div v-if="errors.length !=0">
			<ul v-for="e in errors" :key="e">
				<li>
					<font color="red">{{e}}</font>
				</li>
			</ul>
		</div>
		<div>
			<label>Name</label>
			<input v-model="employee.name" type="text">
			<!-- Vue コンポーネントのデータとフォームデータとの双方向データバインディングが行われます。例えば name に値を入力すると this.employee.name にその値が格納され、this.employee.name に値を入力すると name 用の input フォームにその値が表示されるといった具合 -->
		</div>
		<div>
			<label>Department</label>
			<input v-model="employee.department" type="text">
		</div>
		<div>
			<label>Gender</label>
			<select v-model="employee.gender">
				<option>other</option>
				<option>male</option>
				<option>female</option>
			</select>
		</div>
		<div>
			<label>Birth</label>
			<input v-model="employee.birth" type="date">
		</div>
		<div>
			<label>Joined Date</label>
			<input v-model="employee.joined_date" type="date">
		</div>
		<div>
			<label>Payment</label>
			<input v-model="employee.payment" type="number" min="0">
		</div>
		<div>
			<label>Note</label>
			<input v-model="employee.note" type="text">
		</div>
		<button type="submit">Commit</button>
	</form>
</template>

<script>

export default {
	props: {
		employee: {},
		errors: ''
	}
}
</script>

<style>
	
</style>