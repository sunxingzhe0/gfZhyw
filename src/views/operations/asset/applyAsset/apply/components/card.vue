<template>
	<div>
		<div class="item" v-for="(item, index) in datas" :key="index">
			<div class="item-content">
				<div class="head-title">
					<div class="left">
						{{ item.assetNo }}({{ item.assetType }})
					</div>
					<div class="right">
						{{
							item.applyStatus == 1
								? "待审核"
								: item.applyStatus == 2
								? "已通过"
								: item.applyStatus == 3
								? "已驳回"
								: ""
						}}
					</div>
				</div>
				<div class="head">
					<div class="status bg">
						<svg class="iconfont" aria-hidden="true">
							<use xlink:href="#iconzichanshenling"></use>
						</svg>
					</div>
					<div class="info">
						<div class="name">
							{{ item.assetName }}
						</div>
						<div class="date">{{ item.applyTime }}</div>
						<div class="otherinfo">
							<div class="cycle">
								申请人: {{ item.applyUserName }}
							</div>
							<div class="cycle">
								申领科室: {{ item.applyDeptName }}
							</div>
							<div class="remak">
								申领理由: {{ item.applyRemark }}
							</div>
						</div>
					</div>
				</div>
				<div class="footer" v-if="item.applyStatus == 1">
					<div>
						<el-button type="text" @click="agree(item)"
							>同意</el-button
						>
					</div>
					<div>
						<el-button type="text" @click="refuse(item)"
							>拒绝</el-button
						>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
export default {
	props: {
		datas: {
			type: Array,
		},
	},
	methods: {
		//拒绝
		refuse(item) {
			this.$emit("refuse", item)
		},
		//同意
		agree(item) {
			this.$emit("agree", item)
		},
	},
}
</script>

<style lang="scss" scoped>
.item {
	background: rgba(51, 51, 51, 0.6);
	box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.3);
	margin: 10px 20px;
	.item-content {
		padding: 10px;
		box-sizing: border-box;
	}
	.head-title {
		display: flex;
		justify-content: space-between;
		font-size: 14px;
		padding-bottom: 10px;
		line-height: 14px;
		border-bottom: 1px solid rgba(255, 255, 255, 0.2);
		margin-bottom: 10px;
	}
	.head {
		display: flex;
		.info {
			flex: 1;
			margin-left: 10px;
			overflow: hidden;
			display: flex;
			flex-wrap: wrap;
			.name {
				font-size: 14px;
				line-height: 20px;
				flex: 1;
				margin-top: 0px;
			}
		}
		.otherinfo {
			width: 100%;
			.cycle {
				font-size: 12px;
				color: #b3b3b3;
				margin-top: 10px;
				line-height: 12px;
				width: 50%;
				display: inline-block;
			}
		}
		.remak,
		.date {
			font-size: 12px;
			color: #b3b3b3;
		}
		.status {
			width: 36px;
			height: 36px;
			border-radius: 50%;
			font-size: 18px;
			display: flex;
			justify-content: center;
			align-items: center;
		}
		.nobg {
			background: #666666;
		}
		.bg {
			background: #0071e3;
		}
	}
	.footer {
		text-align: center;
		padding-top: 11px;
		display: flex;
		border-top: 1px solid rgba(255, 255, 255, 0.2);
		margin-top: 10px;
		div {
			flex: 1;
		}
		div {
			border-right: 1px solid rgba(255, 255, 255, 0.2);
		}
		div:last-child {
			border: none;
		}
		.el-button {
			border: none;
			padding: 0px;
		}
	}
}
</style>
