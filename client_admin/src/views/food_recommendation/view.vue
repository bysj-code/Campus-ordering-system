<template>
	<el-main class="bg edit_wrap">
		<el-form ref="form" :model="form" status-icon label-width="120px" v-if="is_view()">

							<el-col v-if="user_group === '管理员' || $check_field('get','product_name') || $check_field('add','product_name') || $check_field('set','product_name')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="餐品名称" prop="product_name">
												<el-input id="product_name" v-model="form['product_name']" placeholder="请输入餐品名称"
							  v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','product_name')) || (!form['food_recommendation_id'] && $check_field('add','product_name'))" :disabled="disabledObj['product_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','product_name')">{{form['product_name']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','picture') || $check_field('add','picture') || $check_field('set','picture')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="图片" prop="picture">
								<el-upload :disabled="disabledObj['picture_isDisabled']" id="picture" class="avatar-uploader" drag
						accept="image/gif, image/jpeg, image/png, image/jpg" action="" :http-request="upload_picture"
						:show-file-list="false" v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','picture')) || (!form['food_recommendation_id'] && $check_field('add','picture'))">
						<img v-if="form['picture']" :src="$fullUrl(form['picture'])" class="avatar">
						<i v-else class="el-icon-plus avatar-uploader-icon"></i>
					</el-upload>
					<el-image v-else-if="$check_field('get','picture')" style="width: 100px; height: 100px"
						:src="$fullUrl(form['picture'])" :preview-src-list="[$fullUrl(form['picture'])]">
						<div slot="error" class="image-slot">
							<img src="../../../public/img/error.png" style="width: 90px; height: 90px" />
						</div>
					</el-image>
							</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','type') || $check_field('add','type') || $check_field('set','type')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="类型" prop="type">
												<el-input id="type" v-model="form['type']" placeholder="请输入类型"
							  v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','type')) || (!form['food_recommendation_id'] && $check_field('add','type'))" :disabled="disabledObj['type_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','type')">{{form['type']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','flavor') || $check_field('add','flavor') || $check_field('set','flavor')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="口味" prop="flavor">
												<el-input id="flavor" v-model="form['flavor']" placeholder="请输入口味"
							  v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','flavor')) || (!form['food_recommendation_id'] && $check_field('add','flavor'))" :disabled="disabledObj['flavor_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','flavor')">{{form['flavor']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','raw_material') || $check_field('add','raw_material') || $check_field('set','raw_material')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="原材料" prop="raw_material">
												<el-input id="raw_material" v-model="form['raw_material']" placeholder="请输入原材料"
							  v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','raw_material')) || (!form['food_recommendation_id'] && $check_field('add','raw_material'))" :disabled="disabledObj['raw_material_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','raw_material')">{{form['raw_material']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','seller_no') || $check_field('add','seller_no') || $check_field('set','seller_no')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="商家编号" prop="seller_no">
																		<div v-if="user_group !== '管理员'">
							{{ get_user_session_seller_no(form['seller_no']) }}
							<!--<el-input id="business_name" v-model="form['seller_no']" placeholder="请输入商家编号"-->
							<!--v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','seller_no')) || (!form['food_recommendation_id'] && $check_field('add','seller_no'))" :disabled="disabledObj['seller_no_isDisabled']"></el-input>-->
							<!--<div v-else-if="$check_field('get','seller_no')">{{form['seller_no']}}</div>-->
							<el-select v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','seller_no')) || (!form['food_recommendation_id'] && $check_field('add','seller_no'))" id="seller_no" v-model="form['seller_no']" :disabled="disabledObj['seller_no_isDisabled']">
								<el-option v-for="o in list_user_seller_no" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
							<el-select v-else-if="$check_field('get','seller_no')" id="seller_no" v-model="form['seller_no']" :disabled="true">
								<el-option v-for="o in list_user_seller_no" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
						</div>
						<el-select v-else id="seller_no" v-model="form['seller_no']" :disabled="disabledObj['seller_no_isDisabled']">
							<el-option v-for="o in list_user_seller_no" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
									   :value="o['user_id']">
							</el-option>
						</el-select>
																</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','store_name') || $check_field('add','store_name') || $check_field('set','store_name')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="店铺名称" prop="store_name">
												<el-input id="store_name" v-model="form['store_name']" placeholder="请输入店铺名称"
							  v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','store_name')) || (!form['food_recommendation_id'] && $check_field('add','store_name'))" :disabled="disabledObj['store_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','store_name')">{{form['store_name']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','detailed_introduction') || $check_field('add','detailed_introduction') || $check_field('set','detailed_introduction')" :xs="24" :sm="24" :lg="24" class="el_form_editor_warp">
				<el-form-item label="详情介绍" prop="detailed_introduction">
					<quill-editor v-model.number="form['detailed_introduction']"
						v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','detailed_introduction')) || (!form['food_recommendation_id'] && $check_field('add','detailed_introduction')) ">
					</quill-editor>
					<div v-else-if="$check_field('get','detailed_introduction')" v-html="form['detailed_introduction']"></div>
				</el-form-item>
			</el-col>
					
	
	

	
	
	
	
			<el-col :xs="24" :sm="12" :lg="8" class="el_form_btn_warp">
				<el-form-item>
					<el-button type="primary" @click="submit()">提交</el-button>
					<el-button @click="cancel()">取消</el-button>
				</el-form-item>
			</el-col>

		</el-form>
	</el-main>
</template>

<script>
	import mixin from "@/mixins/page.js";

	export default {
		mixins: [mixin],
		data() {
			return {
				field: "food_recommendation_id",
				url_add: "~/api/food_recommendation/add?",
				url_set: "~/api/food_recommendation/set?",
				url_get_obj: "~/api/food_recommendation/get_obj?",
				url_upload: "~/api/food_recommendation/upload?",

				query: {
					"food_recommendation_id": 0,
				},

				form: {
								"product_name":  '', // 餐品名称
										"picture":  '', // 图片
										"type":  '', // 类型
										"flavor":  '', // 口味
										"raw_material":  '', // 原材料
										"seller_no": 0, // 商家编号
										"store_name":  '', // 店铺名称
										"detailed_introduction":  '', // 详情介绍
											"food_recommendation_id": 0, // ID
						
				},
				disabledObj:{
								"product_name_isDisabled": false,
										"picture_isDisabled": false,
										"type_isDisabled": false,
										"flavor_isDisabled": false,
										"raw_material_isDisabled": false,
										"seller_no_isDisabled": false,
										"store_name_isDisabled": false,
										"detailed_introduction_isDisabled": false,
										},

	
		
		
		
		
		
					// 用户列表
				list_user_seller_no: [],
						// 用户组
				group_user_seller_no: "",
				
		
	
			}
		},
		methods: {


	
	
						/**
			 * 上传图片
			 * @param {Object} param 图片参数
			 */
			upload_picture(param){
						this.uploadFile(param.file, "picture");
					},
	
	
			
	
			
	
			
	
			
	
				/**
			 * 获取卖家用户列表
			 */
			async get_list_user_seller_no() {
                // if(this.user_group !== "管理员" && this.form["seller_no"] === 0) {
                //     this.form["seller_no"] = this.user.user_id;
                // }
                var json = await this.$get("~/api/user/get_list?user_group=卖家");
                if(json.result && json.result.list){
                    this.list_user_seller_no = json.result.list;
                }
                else if(json.error){
                    console.error(json.error);
                }
			},
					/**
			 * 获取卖家用户组
			 */
			async get_group_user_seller_no() {
							this.form["seller_no"] = this.user.user_id;
							var json = await this.$get("~/api/user_group/get_obj?name=卖家");
				if(json.result && json.result.obj){
					this.group_user_seller_no = json.result.obj;
				}
				else if(json.error){
					console.error(json.error);
				}
			},
			get_user_session_seller_no(id){
				var _this = this;
				var user_id = {"user_id":id}
				var url = "~/api/"+_this.group_user_seller_no.source_table+"/get_obj?"
				this.$get(url, user_id, function(res) {
					if (res.result && res.result.obj) {
						var arr = []
						for (let key in res.result.obj) {
							arr.push(key)
						}
						var arrForm = []
									for (let key in _this.form) {
							arrForm.push(key)
						}
			
									_this.form["seller_no"] = id
									_this.disabledObj['seller_no' + '_isDisabled'] = true
						for (var i=0;i<arr.length;i++){
						  if (arr[i]!=='examine_state' && arr[i]!=='examine_reply') {
							for (var j = 0; j < arrForm.length; j++) {
							  if (arr[i] === arrForm[j]) {
								if (arr[i] !== "seller_no") {
												_this.form[arrForm[j]] = res.result.obj[arr[i]]
												_this.disabledObj[arrForm[j] + '_isDisabled'] = true
									break;
								} else {
								  _this.disabledObj[arrForm[j] + '_isDisabled'] = true
								}
							  }
							}
						  }
						}
					}
				});
			},
					get_user_seller_no(id){
				var obj = this.list_user_seller_no.getObj({"user_id":id});
				var ret = "";
				if(obj){
					if(obj.nickname){
						ret = obj.nickname;}
					else{
						ret = obj.username;
					}
				}
				return ret;
			},
			
	
			
	
		
			/**
			 * 获取对象之前
			 * @param {Object} param
			 */
			get_obj_before(param) {
				var form = "";
											
				if(this.form && form){
					Object.keys(this.form).forEach(key => {
						Object.keys(form).forEach(dbKey => {
							// if(dbKey === "charging_standard"){
							// 	this.form['charging_rules'] = form[dbKey];
							// 	this.disabledObj['charging_rules_isDisabled'] = true;
							// };
							if(key === dbKey){
								this.disabledObj[key+'_isDisabled'] = true;
							}
						})
					})
				}
																				$.db.del("form");
				return param;
			},

			/**
			 * 获取对象之后
			 * @param {Object} json
			 * @param {Object} func
			 */
			get_obj_after(json, func){


																								


			},

			/**
			 * 提交前验证事件
			 * @param {Object} 请求参数
			 * @return {String} 验证成功返回null, 失败返回错误提示
			 */
			submit_check(param) {
				let msg = null
																														return msg;
			},

			is_view(){
				var bl = this.user_group == "管理员";

				if(!bl){
					bl = this.$check_action('/food_recommendation/table','add');
					console.log(bl ? "你有表格添加权限视作有添加权限" : "你没有表格添加权限");
				}
				if(!bl){
					bl = this.$check_action('/food_recommendation/table','set');
					console.log(bl ? "你有表格添加权限视作有修改权限" : "你没有表格修改权限");
				}
				if(!bl){
					bl = this.$check_action('/food_recommendation/view','add');
					console.log(bl ? "你有视图添加权限视作有添加权限" : "你没有视图添加权限");
				}
				if(!bl){
					bl = this.$check_action('/food_recommendation/view','set');
					console.log(bl ? "你有视图修改权限视作有修改权限" : "你没有视图修改权限");
				}
				if(!bl){
					bl = this.$check_action('/food_recommendation/view','get');
					console.log(bl ? "你有视图查询权限视作有查询权限" : "你没有视图查询权限");
				}

				console.log(bl ? "具有当前页面的查看权，请注意这不代表你有字段的查看权" : "无权查看当前页，请注意即便有字段查询权限没有页面查询权限也不行");

				return bl;
			},
			/**
			 * 上传文件
			 * @param {Object} param
			 */
			uploadimg(param) {
				this.uploadFile(param.file, "avatar");
			},


		},
		created() {
															this.get_list_user_seller_no();
					this.get_group_user_seller_no();
									},
	}
</script>

<style>
	.avatar-uploader .el-upload {
		border: 1px dashed #d9d9d9;
		border-radius: 6px;
		cursor: pointer;
		position: relative;
		overflow: hidden;
	}

	.avatar-uploader .el-upload:hover {
		border-color: #409EFF;
	}

	.avatar-uploader-icon {
		font-size: 28px;
		color: #8c939d;
		width: 178px;
		height: 178px;
		line-height: 178px;
		text-align: center;
	}

	.avatar {
		width: 178px;
		height: 178px;
		display: block;
	}




</style>
