<template>
	<div>
		<div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-edit"></i> 个人中心</el-breadcrumb-item>
                <el-breadcrumb-item>修改用户</el-breadcrumb-item>
            </el-breadcrumb>
		</div>
		<div class="userContent">
			<el-form ref="form" :model="form" :rules="rules" label-width="80px">
				<el-form-item prop="name" label="用户名称">
					<el-input v-model="form.name" disabled></el-input>
				</el-form-item>
				<el-form-item prop="account" label="账号名称">
					<el-input v-model="form.account" disabled></el-input>
				</el-form-item>
				<el-form-item prop="email" label="邮箱">
					<el-input v-model="form.email" placeholder="请输入邮箱"></el-input>
				</el-form-item>
				<el-form-item prop="phone" label="手机">
					<el-input v-model="form.phone" placeholder="请输入手机号"></el-input>
				</el-form-item>
				<el-form-item prop="card" label="身份证">
					<el-input v-model="form.card" placeholder="请输入身份证"></el-input>
				</el-form-item>
				<el-form-item prop="birth" label="出生日期">
					<el-col :span="24">
						<el-date-picker type="date" placeholder="选择日期" v-model="form.birth" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
					</el-col>
				</el-form-item>
				<el-form-item prop="sex" label="性别">
					<el-select class="select-sex" v-model="form.sex" placeholder="请选择性别">
						<el-option label="男" value="man"></el-option>
						<el-option label="女" value="woman"></el-option>
					</el-select>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" @click="updateUserData('form')">确定</el-button>
					<el-button @click="onCancle()">取消</el-button>
				</el-form-item>
			</el-form>
		</div>
	</div>
</template>

<script>
	import Util from '../../utils/utils';
	export default {
		data() {
			var validateEmail = (rule, value, callback) => {
				if (value === '') {
					callback(new Error('请输入邮箱'));
				} else if (!Util.emailReg.test(this.form.email)){
					callback(new Error('请输入正确的邮箱'));
				} else {
					callback();
				}
			};
			var validatePhone = (rule, value, callback) => {
				if (value === '') {
					callback(new Error('请输入手机号'));
				} else if (!Util.phoneReg.test(this.form.phone)){
					callback(new Error('请输入正确的手机号'));
				} else {
					callback();
				}
			};
			var validateCard = (rule, value, callback) => {
				if (value === '') {
					callback(new Error('请输入身份证号'));
				} else if (!Util.idCardReg.test(this.form.card)){
					callback(new Error('请输入正确的身份证号'));
				} else {
					callback();
				}
			};
            return {
				form: {
					name: '',
					account: '',					
					email: '',
					phone: '',
					card: '',
					birth: '',
					sex: ''
				},
				rules: {
                    name: [
                        { required: true, message: '请输入用户名', trigger: 'blur' }
                    ],
                    account: [
                        { required: true, message: '请输入账号', trigger: 'blur' }
                    ],
                    email: [
                        { validator: validateEmail, trigger: 'blur' }
                    ],
                    phone: [
                        { validator: validatePhone, trigger: 'blur' }
                    ],
                    card: [
                        { validator: validateCard, trigger: 'blur' }
                    ],
                    birth: [
                        { required: true, message: '请输入出生日期',type: 'date', trigger: 'blur' }
                    ],
                    sex: [
                        { required: true, message: '请输入性别', trigger: 'blur' }
                    ]
                }
			}
        },
        methods:{
        	getUserData() {
				const self = this;	
				let username = sessionStorage.getItem('ms_user').name;			
				self.$http.get('/api/user/getUser',{name: username}).then(function(response) {
					console.log(response);
					let result = response.data[0];
					self.form.name = result.username;
					self.form.account = result.account;
					self.form.email = result.email;
					self.form.phone = result.phone;
					self.form.card = result.card;
					self.form.birth = new Date(result.birth);
					self.form.sex = result.sex;
					sessionStorage.setItem('ms_userId', result.id);
				}).then(function(error) {
					console.log(error);
				})
			},			
			updateUserData(formName) {
				const self = this;
				let formData = {
					id: parseInt(sessionStorage.getItem('ms_userId')),
					email: self.form.email,
					phone: self.form.phone,
					card: self.form.card,
					birth: self.form.birth,
					sex: self.form.sex
				};
						
				self.$refs[formName].validate((valid) => {
                    if (valid) {
                        self.$http.post('/api/user/updateUser',formData).then(function(response) {
							console.log(response);										
							self.$router.push('/success');
						}).then(function(error) {
							console.log(error);
						})
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
			},
        	onCancle() {
        		 this.$router.push('/userCenter');
        	}       	
		},
		//初始化
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