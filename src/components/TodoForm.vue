<template>
	<div class="todoForm">
		<div class="taskPanel">
			<div class="todoFormTop">
				<input type="text" placeholder="Please Type Something Here..." v-model="taskTitle">
			</div>
			<div class="formGroup">
				<h4>Deadline</h4>
				<input type="date" v-model="deadlineDate">
				<input type="time" v-model="deadlineTime">
				<h4>Comment</h4>
				<textarea rows="10" v-model="comment"></textarea>
			</div>
		</div>	
		<div class="btnGroup">
			<button class="btnAdd" v-on:click="addTask">Add</button>
			<button  class="btnCancel">Cancel</button>
		</div>
	</div>
</template>
	
<script>
export default {
	name: 'TodoForm',
	data() {
		return {
			taskTitle: '',
			deadlineDate: '',
			deadlineTime: '',
			comment: ''
		}
	},
	methods: {
		addTask() {
			if (this.taskTitle !== '') {
				this.$emit('todoTask', {
					title: this.taskTitle,
					deadline: {
						date: this.deadlineDate,
						time: this.deadlineTime
					},
					comment: this.comment,
					isFinished: false,
					isShow: false,
					isUpdate: false,
					isMark: false
				});
				this.taskTitle = '';
				this.deadlineDate = '';
				this.deadlineTime = '';
				this.comment = '';
			}
		}
	}
}
</script>

<style>
	.todoForm {
		width: 100%;
		max-width: 500px;
		margin: 0 auto;
	}
	.todoForm .taskPanel {
		background-color: #F2F2F2;
		padding: 0 0 15px;
	}
	.todoForm .taskPanel .todoFormTop {
		text-align: center;
	}
	.todoForm .todoFormTop input[type="text"] {
		background-color: #F2F2F2;
		width: 90%;
		height: 50px;
		border: 0;
		font-size: 24px;
		font-weight: 500;
	}
	.todoForm .taskPanel .formGroup {
		border-top: 2px solid #c8c8c8;
		padding: 0px 20px;
		text-align: left;
	}
	.todoForm .formGroup h4 {
		text-align: left;
		color: #000;
		font-size: 20px;
		font-weight: 500;
		letter-spacing: 1px;
		margin: 15px 0 10px;
	}
	.todoForm .formGroup input[type='date'],
	.todoForm .formGroup input[type='time'] {
		width: 180px;
		margin-right: 10px;
		padding: 10px 15px;
		font-size: 16px;
	}
	.todoForm .formGroup textarea {
		width: 100%;
		padding: 8px;
	}
	.todoForm .btnGroup {
		display: flex;
	}
	.todoForm .btnGroup button {
		background-color: #fff;
		width: 50%;
		height: 60px;
		font-size: 24px;
		padding: 15px 0;
		border: 0;
		cursor: pointer;
	}
	.todoForm .btnGroup .btnAdd {
		background: #4A90E2;
		color: #fff;
		border-bottom-left-radius: 10px;
	}
	.todoForm .btnGroup .btnCancel {
		background: #fff;
		color: #D0021B;
		border-bottom-right-radius: 10px;
	}
	@media screen and (max-width: 500px) {
		.todoForm .btnGroup .btnCancel {
			border-bottom-right-radius: 0px;
		}
		.todoForm .btnGroup .btnAdd {
			border-bottom-left-radius: 0px;
		}
	}
	@media screen and (max-width: 424px) {
		.todoForm .formGroup input[type='time'] {
			margin: 5px 10px 0 0;
		}
	}
	
</style>