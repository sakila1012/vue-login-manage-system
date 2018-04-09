<template>
	<div class="main-content">
		<div class="crumbs">
			<el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i>&nbsp;&nbsp;修改成功</el-breadcrumb-item>
            </el-breadcrumb>
		</div>
		<div class="eidt-success">
			<div>
				<i class="el-icon-check"></i>&nbsp;&nbsp;<span>恭喜您，修改成功</span>
			</div>            	
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
		}	
    }
	
</script>

<style>
	.main-content{
		background-image: url('../../../static/img/success.jpg');
		background-size: cover;
		height: 100%;
	}
	.eidt-success {
		width: 400px;
		margin: 0 auto;
		position: relative;
		
	}
	.eidt-success div {
		position: absolute;
		margin-left: -190px;
		left: 50%;
	}
	.eidt-success i {
		color: #67C23A;
	}
	.eidt-success i,.eidt-success span {
		font-size: 40px;
		font-family: Microsoft YaHei;
	}
</style>