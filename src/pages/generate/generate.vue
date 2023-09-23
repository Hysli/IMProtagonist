<template>
	<view class="container">
		<view class="header">
			<h1>请上传1~5张自拍照🤳🏻</h1>
		</view>
		<view class="body">
			<uni-card>
				<uni-file-picker v-model="imageValue" fileMediatype="image" mode="grid" :limit="5" @select="select" @progress="progress" @success="success" @fail="fail" />
				<br />
				<uni-forms ref="valiForm" :rules="rules" :modelValue="formData" label-position="left">
					<uni-forms-item required name="prompt">
						<uni-easyinput class="form-input" type="textarea" v-model="formData.prompt" placeholder="请输入prompt" />
					</uni-forms-item>
					<uni-forms-item name="gender" required>
						<uni-data-checkbox v-model="formData.gender" :localdata="sexs" />
					</uni-forms-item>
					<uni-forms-item>
						<button class="sub-btn" @click="submit()">Generate</button>
					</uni-forms-item>
				</uni-forms>
				<view class="footer" v-show="imageValue.length">
					<uni-list>
						<template v-for="item in imageValue" :key="item">
							<uni-list-item :title="item"></uni-list-item>
						</template>
					</uni-list>
				</view>
			</uni-card>
		</view>
	</view>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface ImageInfo {
	name: string;
	path: string;
	size: number;
}
const sexs = [
	{
		text: '男',
		value: 0
	},
	{
		text: '女',
		value: 1
	}
];
const valiForm = ref();
const formData = ref({
	prompt: ''
});
const rules = {
	prompt: {
		rules: [
			{
				required: true,
				errorMessage: 'prompt不能为空'
			}
		]
	},
	gender: {
		rules: [
			{
				required: true,
				errorMessage: '性别不能为空' }
		]
	}
};
const imageValue = ref<ImageInfo[]>([]);

// 获取上传状态
function select(e) {
	const tempFiles = e.tempFiles;
	for (let i = 0; i < tempFiles.length; i++) {
		imageValue.value.push({
			name: tempFiles[i].name,
			path: tempFiles[i].path,
			size: tempFiles[i].size
		});
	}
}
// 获取上传进度
function progress(e) {
	console.log('上传进度：', e);
}

// 上传成功
function success(e) {
	console.log('上传成功');
}

// 上传失败
function fail(e) {
	console.log('上传失败：', e);
}

function submit() {
	valiForm.value
		.validate()
		.then(() => {
			if (imageValue.value.length) {
				uni.showToast({
					title: '通过',
					icon: 'success',
					mask: true,
					duration: 500
				}).then(() => {
					uni.navigateTo({
						url: '../result/result'
					});
				});
			} else {
				uni.showToast({
					title: '请上传图片',
					icon: 'error',
					mask: true,
					duration: 500
				});
			}
		})
		.catch((err) => {
			console.error('错误', err[0].errorMessage);
		});
}
</script>

<style lang="scss" scoped>
.container {
	overflow: hidden;
}
.header {
	text-align: center;
	margin: 20px 0;
}
.from-card {
	padding: 10px;
	height: 300px;
}
::v-deep .form-input .is-input-border {
	border: 2px solid #000 !important;
	& .uni-easyinput__content-textarea {
		height: 150px;
	}
}
.sub-btn {
	width: 10%;
	background-color: rgb(137, 28, 227);
	color: #fff;
	border-radius: 999px;
}
::v-deep .file-picker__box {
	width: 400px !important;
	padding-top: 400px;
}
</style>
