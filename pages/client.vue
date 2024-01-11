<template>
	<view class="content">
		<uni-easyinput v-model="msg" type="textarea" auto-height placeholder="请输入内容"></uni-easyinput>
		<button @click="onSendClick">发送消息</button>
		<view class="box">
			<view v-for="item in messages" class="item">服务器[{{ item.host }}]回复消息：{{ item.msg }}</view>
		</view>
	</view>
</template>

<script>
	import {
		UDPClient
	} from '@/uni_modules/uts-udp';

	export default {
		data() {
			return {
				msg: '',
				messages: []
			}
		},
		methods: {
			onSendClick() {
				UDPClient.send({
					host: '255.255.255.255',
					port: 7000,
					receiveTimeout: 2000,
					msg: this.msg,
					enableRecive: true,
					onceReceive: (data) => {
						this.messages.push(data);
					},
					onError: (error) => {
						console.error(error);
					},
					onceReceiveTimeout: () => {
						console.warn('服务器超时未回复');
					},
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		font-size: 15px;
	}

	.box {
		padding: 10px;
	}
</style>