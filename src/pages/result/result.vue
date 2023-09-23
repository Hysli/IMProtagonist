<template>
	<view>
		<view class="header">
			<h1>请输入邮箱用来接受生成完成 的通知</h1>
		</view>
		<view class="body">
			<uni-card>
				<uni-forms ref="valiForm" :rules="rules" :modelValue="formData" label-position="left">
					<uni-forms-item name="email">
						<uni-easyinput class="input" v-model="formData.email" type="text" placeholder="请输入邮箱" />
					</uni-forms-item>
					<uni-forms-item>
						<button class="sub-btn" @click="submit()">Generate</button>
					</uni-forms-item>
				</uni-forms>
			</uni-card>
		</view>
	</view>
</template>

<script setup lang="ts">
import { ref } from 'vue';
const valiForm = ref();
const formData = ref({
	email: ''
});
const rules = {
	email: {
		rules: [
			{
				required: true,
				errorMessage: 'email不能为空'
			},
			{
				validateFunction: function (rule, value, data, callback) {
					if (
						/^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(
							value
						)
					) {
						return true;
					}
					callback('邮箱格式错误');
				}
			}
		]
	}
};
function submit() {
	valiForm.value
		.validate()
		.then(() => {
			uni.showToast({
				title: '通过',
				icon: 'success',
				mask: true,
				duration: 500
			});
		})
		.catch((err) => {
			console.error('错误', err[0].errorMessage);
		});
}
</script>

<style lang="scss">
.header {
	text-align: center;
	margin: 20px 0;
}
.sub-btn {
	width: 10%;
	background-color: rgb(137, 28, 227);
	color: #fff;
	border-radius: 999px;
}
</style>
