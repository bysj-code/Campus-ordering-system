<template>
	<view class="diy_details diy_div_food_recommendation">
		<view v-if="Object.keys(obj).length!==0" class="warp">
			<view class="container">
				<view class="row">
					<view v-if="$check_field('get','product_name')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>餐品名称:</span>
						</view>
							<view class="diy_field diy_text">
							<text>
								{{obj["product_name"]}}
							</text>
						</view>
						</view>
					<view v-if="$check_field('get','picture')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>图片:</span>
						</view>
							<view class="diy_field diy_img">
							<image style="width:100%;height:auto;" :src="$fullUrl(obj['picture'])" mode="widthFix"></image>
						</view>
						</view>
					<view v-if="$check_field('get','type')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>类型:</span>
						</view>
							<view class="diy_field diy_text">
							<text>
								{{obj["type"]}}
							</text>
						</view>
						</view>
					<view v-if="$check_field('get','flavor')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>口味:</span>
						</view>
							<view class="diy_field diy_text">
							<text>
								{{obj["flavor"]}}
							</text>
						</view>
						</view>
					<view v-if="$check_field('get','raw_material')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>原材料:</span>
						</view>
							<view class="diy_field diy_text">
							<text>
								{{obj["raw_material"]}}
							</text>
						</view>
						</view>
					<view v-if="$check_field('get','seller_no')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>商家编号:</span>
						</view>
							<view class="diy_field diy_uid">
							<text>
								{{ get_user_seller_no(obj['seller_no']) }}
							</text>
						</view>
						</view>
					<view v-if="$check_field('get','store_name')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>店铺名称:</span>
						</view>
							<view class="diy_field diy_text">
							<text>
								{{obj["store_name"]}}
							</text>
						</view>
						</view>
					<view v-if="$check_field('get','detailed_introduction')" class="col-12 col-md-6">
						<view class="diy_title">
							<span>详情介绍:</span>
						</view>
							<view class="diy_field diy_desc">
							<text>
								{{ obj["detailed_introduction"] }}
							</text>
						</view>
						</view>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	import mixin from "@/libs/mixins/component.js";
	export default {
		mixins: [mixin],
		props: {
			obj_goods: {
				type: Object,
				default(){
					return {}
				}
			},
			query:{
				type: Object,
				default(){
					return {
						food_recommendation_id: 0
					}
				}
			}
		},
		data() {
			return {
				obj_goods_type: {},
				obj: {},
										// 用户列表
				list_user_seller_no: [],
						}
		},
		methods: {
			async get_obj_goods_type() {
				var res = await this.$get("~/api/goods_type/get_obj", {
					name: this.obj_goods.type
				})

				if (res.result.obj) {
					this.obj_goods_type = res.result.obj
				} else {
					console.log("没有请求到商品分类");
				}
			},
			async get_obj_by_goods() {
				var {
					source_table,
					source_field
				} = this.obj_goods_type
				var {
					source_id
				} = this.obj_goods
				var query = {}
				query[source_field] = source_id
				this.$get("~/api/" + source_table + "/get_obj", {}, (res) => {
					if (res.result.obj) {
						this.obj = res.result.obj
					} else {
						console.log("没有请求到商品分类");
					}
				})
			},
			async get_obj_by_id(){
				var res = await this.$get("~/api/food_recommendation/get_obj", {
					food_recommendation_id:this.query.food_recommendation_id
				})

				if (res.result && res.result.obj) {
					this.obj = res.result.obj
				} else {
					console.log("没有请求到商品分类");
				}
			},
									/**
			 * 获取卖家用户列表
			 */
			async get_list_user_seller_no() {
				var json = await this.$get("~/api/user/get_list?user_group=卖家");
				if(json.result && json.result.list){
					this.list_user_seller_no = json.result.list;
				}
				else if(json.error){
					console.error(json.error);
				}
			},
			get_user_seller_no(id){
				let obj = this.list_user_seller_no;
				let ret = "";
				for(let i=0;i<obj.length;i++){
					if(obj[i].user_id==id){
						ret = obj[i].nickname+"-"+obj[i].username;
					}
				}
				return ret;
			},
					},
		created() {
									this.get_list_user_seller_no();
					},
		async onLoad(){
			if (Object.keys(this.obj_goods).length !== 0) {
				await this.get_obj_goods_type();
				await this.get_obj_by_goods();
			} else if (this.query["food_recommendation_id"] !==0) {
				await this.get_obj_by_id();
			}
		}
	}
</script>

<style scoped>
	.div_food_recommendation_details {}

	.details {
		background-color: #fff;
		margin-bottom: 0.5rem;
		padding: 1rem;
		font-size: 10px;
	}

	.item {
		display: flex;
		padding: 0.2rem 0;
		border-bottom: 1px solid #eee;
	}

	.left_text {
		flex: 0 0 25%;
	}

	.right_text {
		flex: 0 0 75%;
	}
	.edit_nav{
		text-align: center;
		background-color: #fff;
		padding: 0.3rem;
		margin: 0.1rem 1rem;
		border: 1px solid #eee;
		border-radius: 0.5rem;
	}
</style>
