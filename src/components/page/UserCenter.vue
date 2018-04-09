<template>
	<div>
		<div class="crumbs">
			<el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-setting"></i>个人中心</el-breadcrumb-item>
            </el-breadcrumb>
		</div>
		<div class="userContent">
			<el-form ref="form" :model="form" label-width="80px">
				<el-form-item label="用户名称">
					<el-input v-model="form.name"></el-input>
				</el-form-item>
				<el-form-item label="账号名称">
					<el-input v-model="form.account"></el-input>
				</el-form-item>
				<el-form-item label="邮箱">
					<el-input v-model="form.email"></el-input>
				</el-form-item>
				<el-form-item label="手机">
					<el-input v-model="form.phone"></el-input>
				</el-form-item>
				<el-form-item label="身份证">
					<el-input v-model="form.card"></el-input>
				</el-form-item>
				<el-form-item label="出生日期">
					<el-col :span="24">
						<el-date-picker type="date" placeholder="选择日期" v-model="form.birth" style="width: 100%;"></el-date-picker>
					</el-col>
				</el-form-item>
				<el-form-item label="性别">
					<el-select class="select-sex" v-model="form.sex" placeholder="请选择性别">
						<el-option label="男" value="man"></el-option>
						<el-option label="女" value="woman"></el-option>
					</el-select>
				</el-form-item>
			</el-form>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
            return {
				form: {

				}
			}
		},
		methods:{
			getUserData() {
				const self = this;	
				let username = localStorage.getItem('ms_user').name;			
				self.$http.get('/api/user/getUser',{name: username}).then(function(response) {
					console.log(response);
					let result = response.data[0];
					self.form.name = result.username;
					self.form.account = result.account;
					self.form.email = result.email;
					self.form.phone = result.phone;
					self.form.card = result.card;
					self.form.birth = result.birth;
					self.form.sex = result.sex;
				}).then(function(error) {
					console.log(error);
				})
			}				
		},
		mounted() {
			this.getUserData();
		}     	
    }
	
</script>

<style>
	.userContent {
		width: 400px;
		margin: 0 auto;
	}
	.select-sex {
		width: 320px;
	}
</style>