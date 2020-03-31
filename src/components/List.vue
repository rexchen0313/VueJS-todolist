<template>
  <div class="taskList">
		<ul>
			<li 
        v-for="(task, index) in todoList"
        v-bind:key="index"
      >
				<div class="taskItem" v-bind:class="{mark:task.isMark}">
					<input type="checkbox" v-model="task.isFinished">
					<input
						type="text"
						v-bind:class="{finish:task.isFinished}"
						v-bind:disabled='!task.isUpdate'
						v-model="task.title"
					>
					<button class="btnMark" v-on:click="markTask(index)">標記</button>
					<button class="btnUpdate" v-on:click="showTask(index)">
						{{ task.isShow ? '關閉' : '顯示'}}
					</button>
					<button class="btnDel" v-on:click="delTask(index)">刪除</button>
				</div>
				
				<div class="taskPanel" v-if="task.isShow">
					<div class="panelForm">
						<h4>Deadline</h4>
						<input
							type="date"
							v-model="task.deadline.date"
							v-bind:disabled='!task.isUpdate'
						>
						<input 
							type="time"
							v-model="task.deadline.time"
							v-bind:disabled='!task.isUpdate'
						>
						<h4>Comment</h4>
						<textarea 
							rows="10"
							v-model="task.comment"
							v-bind:disabled='!task.isUpdate'
						></textarea>
					</div>
					<button class="btnSave" v-on:click="updateTask(index)">
						{{ task.isUpdate ? '保存' : '修改'}}
					</button>
					<button class="btnCancel" v-on:click="cancelChangeTask(index)">取消</button>
				</div>
      </li>
		</ul>
	</div>
</template>

<script>

export default {
	name: "List",
	components: {},
	data() {
		return {
			targetIndex: -1,
			temp : {
				index: '',
				title: '',
				deadline: {
					date: '',
					time: ''
				},
				comment: ''
			}
		}
	},
	props: { // 父元件向子元件傳遞
		todoList: {
			type: Array,
			default: []
		}
	},
	methods: {
		markTask(index){
			this.todoList[index].isMark = !this.todoList[index].isMark;
		},
		delTask(index) {
			this.todoList.splice(index, 1);
		},
		showTask(index) {
			this.todoList[index].isShow = !this.todoList[index].isShow;
			if(this.todoList[index].isShow) {
				this.targetIndex = index;
				for(let i = 0; i < this.todoList.length; i++) {
					if(i !== this.targetIndex) {
						this.todoList[i].isShow = false;
						this.todoList[i].isUpdate = false;
					}
				}
			} else {
				this.todoList[index].isUpdate = false;
				this.targetIndex = -1;
			}
		},
		updateTask(index) {
			this.todoList[index].isUpdate = !this.todoList[index].isUpdate;
			if(this.todoList[index].isUpdate) { // 按下修改時執行
				this.temp.index = index;
				this.temp.title = this.todoList[index].title;
				this.temp.deadline.date = this.todoList[index].deadline.date;
				this.temp.deadline.time = this.todoList[index].deadline.time;
				this.temp.comment = this.todoList[index].comment;
			} else {
				this.temp.index = '';
				this.temp.title = '';
				this.temp.deadline.date = '';
				this.temp.deadline.time = '';
				this.temp.comment = '';
			}
		},
		cancelChangeTask(index) {
			if(this.temp.index === index) {
				this.todoList[index].title = this.temp.title;
				this.todoList[index].deadline.date = 	this.temp.deadline.date;
				this.todoList[index].deadline.time = this.temp.deadline.time;
				this.todoList[index].comment = this.temp.comment;
			} else {
				this.temp.index = '';
				this.temp.title = '';
				this.temp.deadline.date = '';
				this.temp.deadline.time = '';
				this.temp.comment = '';
			}
			this.todoList[index].isShow = !this.todoList[index].isShow;
			if(!this.todoList[index].isShow){
				this.todoList[index].isUpdate = false;
			}
			
		}
	},
}
</script>

<style>
	.taskList {
		width: 100%;
		max-width: 500px;
		margin: 15px auto 0;
	}
	.taskList ul li {
		margin-bottom: 15px;
		list-style: none;
		font-size: 24px;
		font-weight: 500;
		overflow: hidden;
	}
	.taskList .taskItem {
		background-color: #f2f2f2;
		width: 100%;
		padding: 10px 20px;
		font-size: 0;
		border-radius: 8px;
	}
	.taskList .taskItem input[type='checkbox'] {
		margin: 0 15px 0 5px;
	}
	.taskList .taskItem input[type='text'] {
		background-color: #e4e4e4;
		width: 400px;
		height: 50px;
		margin: 10px 0;
		padding-left: 10px;
		font-size: 24px;
		border: 0;
		border-radius: 8px;
		text-overflow: ellipsis
	}
	.taskList .taskItem .finish {
		color: gray;
		text-decoration: line-through;
	}
	.taskList .mark {
		background-color: #fff2dc;
	}
	.taskList .taskItem button {
		width: 33.3%;
		height: 40px;
		border: 0;
		font-size: 18px;
		font-weight: 500;
		cursor: pointer;

	}
	.taskList .taskItem .btnUpdate {
		background-color: #4a90e2;
		color: #fff;
	}
	.taskList .taskItem .btnDel {
		background-color: #d0021b;
		color: #fff;
	}
	.taskList .taskItem .btnMark {
		background-color: #f5a623;
		color: #fff;
	}
	.taskList	.taskPanel {
		background-color: #F2F2F2;
		border-top: 2px solid #c8c8c8;
		font-size: 0;
	}
	.taskList .taskPanel .panelForm {
		padding: 0px 20px;
		margin-bottom: 10px;
	}
	.taskList	.taskPanel h4 {
		margin: 15px 0;
		color: #000;
		font-size: 20px;
		font-weight: 500;
	}
	.taskList	.taskPanel input[type='date'],
	.taskList	.taskPanel input[type='time'] {
		width: 180px;
		margin-right: 10px;
		padding: 10px 15px;
		font-size: 16px;
	}
	.taskList .taskPanel textarea {
		width: 100%;
		padding: 8px;
	}
	.taskList .taskPanel button{
		width: 50%;
		height: 40px;
		border: 0;
		font-size: 18px;
		font-weight: 500;
		cursor: pointer;
	}
	.taskList .taskPanel .btnSave {
		background-color: #4a90e2;
		color: #fff;
	}
	.taskList .taskPanel .btnCancel {
		background-color: #fff;
		color: #d0021b;
	}
	@media screen and (max-width: 500px) {
		.taskList .taskItem input[type='text'] {
			width: 100%;
			margin: 10px 0;
		}
		.taskList .taskItem {
			border-radius: 0px;
		}
		.taskList .taskItem input[type='text'] {
			border-radius: 0px;
		}
	}
</style>