<template>
	<div class="contract-list">
		<div class="list-left">
			<div class="title">供应商列表</div>
			<div class="table-main">
				<el-input
					placeholder="关键字"
					size="mini"
					v-model="query.keyword"
					class="input-with-select"
				>
					<el-button
						slot="append"
						icon="el-icon-search"
						type="primary"
						size="mini"
						@click="search"
					></el-button>
				</el-input>
				<Table
					v-model="query"
					:columns="columns"
					:tableData="tableData"
					:bats="[]"
				>
				</Table>
			</div>
		</div>
		<div class="list-btn">
			<el-button plain class="addBtn" size="mini" @click="setLians"
				><i class="el-icon-right"></i
			></el-button>
			<el-button plain class="removeBtn" size="mini" @click="removeLians"
				><i class="el-icon-back"></i
			></el-button>
		</div>
		<div class="list-right">
			<div class="title">黑名单供应商</div>
			<supplerList ref="table2"></supplerList>
		</div>
	</div>
</template>
<script>
import { Table, TableMixin } from "@/components/App"
import asset from "@/api/operations/asset"
import supplerList from "./form/supplerList"
export default {
	components: {
		Table,
		supplerList,
	},
	mixins: [
		TableMixin([
			{
				fetchListFunction: asset.blacklist.getsupplierList,
			},
		]),
	],
	data() {
		return {
			query: {
				pageNum: 1,
				pageSize: 10,
				keyword: "",
			},
			columns: {
				index: {
					width: "10px",
					hidden: true,
				},
			},
		}
	},
	methods: {
		search() {
			this.$_fetchTableData(asset.blacklist.getsupplierList)
		},
		async setLians() {
			let ids = []
			const list = this.tableData.multipleSelection
			if (list.length === 0) {
				this.$message.warning("请先选择要添加黑名单的供应商")
				return false
			}
			for (const i in list) {
				ids.push(list[i].id)
			}
			await asset.blacklist.saveBlacklist({ supplierIds: ids.join(",") })
			this.$message.success("操作成功")
			this.search()
			this.$refs.table2.$_fetchTableData(asset.blacklist.getsupplierList)
		},
		async removeLians() {
			let ids = []
			const list = this.$refs.table2.tableData.multipleSelection
			if (list.length === 0) {
				this.$message.warning("请先选择要移除黑名单的供应商")
				return false
			}
			for (const i in list) {
				ids.push(list[i].id)
			}
			await asset.blacklist.removeBlacklist({
				supplierIds: ids.join(","),
			})
			this.$message.success("操作成功")
			this.search()
			this.$refs.table2.$_fetchTableData(asset.blacklist.getsupplierList)
		},
	},
}
</script>
<style lang="scss">
.contract-list {
	display: flex;
	height: 550px;
	padding: 0px 20px 20px 20px;
	box-sizing: border-box;
	.list-left,
	.list-right {
		width: 45%;
		position: relative;
		height: 100%;
		background: rgba(26, 26, 26, 0.8);
		padding-top: 10px;
		.title {
			position: absolute;
			left: 0px;
			top: 0px;
			font-size: 16px;
			font-weight: 500;
			color: #fff;
			margin: 10px 20px;
			padding: 6px 0px;
			padding-left: 10px;
			line-height: 16px;
			&::before {
				content: "";
				position: absolute;
				left: 0;
				top: 6px;
				width: 2px;
				height: 16px;
				background: #0071e3;
			}
		}
	}
	.list-btn {
		width: 10%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		.el-button {
			width: 60px;
			display: block;
			margin-left: 0;
			margin-bottom: 20px;
			padding: 7px 7px;
		}
		.addBtn {
			border: 1px solid #0071e3;
			color: #0071e3;
			background: transparent;
		}
		.removeBtn {
			border: 1px solid #f74a4a;
			color: #f74a4a;
			background: transparent;
		}
	}
	.table-main {
		height: 100%;
		.input-with-select {
			width: 260px;
			float: right;
			margin-bottom: 10px;
			margin-right: 10px;
			.el-input-group__append {
				width: 26px !important;
				color: #0071e3;
				background: transparent;
				border-color: #4d4d4d;
				padding: 0px;
				.el-button {
					margin: 0;
					width: 100%;
					height: 100%;
					padding: 0;
				}
			}
			.el-input-group__prepend {
				width: 100px !important;
				color: #0071e3;
				background: transparent;
				border-color: #4d4d4d;
			}
		}
		.c__list {
			height: 100%;
			& > .el-table,
			.DarkDialog .c__list .el-table {
				height: calc(100% - 96px) !important;
			}
		}
	}
}
</style>
