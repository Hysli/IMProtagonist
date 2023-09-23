<template>
	<view class="container">
		<view class="header">
			<h1>facechina video</h1>
		</view>
		<view class="body">
			<uni-card class="card">
				<template v-for="item in previewImgList" :key="item">
					<image :src="item" mode="aspectFill"></image>
				</template>
			</uni-card>
			<button class="sub-btn" @click="submit()">Let's go~</button>
		</view>
	</view>
</template>

<script setup lang="ts">
import { ref } from 'vue';
const previewImgList = [
	'../../static/image/1.png',
	'../../static/image/2.png',
	'../../static/image/3.png',
	'../../static/image/4.png',
	'../../static/image/5.png',
	'../../static/image/6.png'
];

function submit() {
	uni.navigateTo({
		url: '../generate/generate'
	});
}
</script>

<style lang="scss" scoped>
$scale: 0.9;
.header {
	text-align: center;
	margin: 20px 0;
}
.container {
	min-width: 1990px;
	// height: 100vh;
	overflow: hidden;
}

.body {
	height: 100%;
}
uni-image {
	width: $scale * 350px;
	height: $scale * 700px;
	cursor: pointer;
	transform: scale(1);
	transition: transform 0.3s;
}
uni-image:hover {
	transform: scale(1.2);
}
::v-deep .card .uni-card__content {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-around;
}
.sub-btn {
	margin-top: 100px;
	width: 10%;
	background-color: rgb(137, 28, 227);
	color: #fff;
	border-radius: 999px;
}
</style>
