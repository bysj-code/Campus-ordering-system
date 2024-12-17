<template>
	<el-main class="bg edit_wrap">
		<el-form ref="form" :model="form" status-icon label-width="120px" v-if="is_view()">

							<el-col v-if="user_group === '管理员' || $check_field('get','title') || $check_field('add','title') || $check_field('set','title')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="标题" prop="title">
												<el-input id="title" v-model="form['title']" placeholder="请输入标题"
							  v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','title')) || (!form['business_analysis_id'] && $check_field('add','title'))" :disabled="disabledObj['title_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','title')">{{form['title']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','seller_no') || $check_field('add','seller_no') || $check_field('set','seller_no')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="商家编号" prop="seller_no">
																		<div v-if="user_group !== '管理员'">
							{{ get_user_session_seller_no(form['seller_no']) }}
							<!--<el-input id="business_name" v-model="form['seller_no']" placeholder="请输入商家编号"-->
							<!--v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','seller_no')) || (!form['business_analysis_id'] && $check_field('add','seller_no'))" :disabled="disabledObj['seller_no_isDisabled']"></el-input>-->
							<!--<div v-else-if="$check_field('get','seller_no')">{{form['seller_no']}}</div>-->
							<el-select v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','seller_no')) || (!form['business_analysis_id'] && $check_field('add','seller_no'))" id="seller_no" v-model="form['seller_no']" :disabled="disabledObj['seller_no_isDisabled']">
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
							  v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','store_name')) || (!form['business_analysis_id'] && $check_field('add','store_name'))" :disabled="disabledObj['store_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','store_name')">{{form['store_name']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','statistical_time') || $check_field('add','statistical_time') || $check_field('set','statistical_time')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="统计时间" prop="statistical_time">
												<el-input id="statistical_time" v-model="form['statistical_time']" placeholder="请输入统计时间"
							  v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','statistical_time')) || (!form['business_analysis_id'] && $check_field('add','statistical_time'))" :disabled="disabledObj['statistical_time_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','statistical_time')">{{form['statistical_time']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','total_revenue') || $check_field('add','total_revenue') || $check_field('set','total_revenue')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="收入总额" prop="total_revenue">
								<el-input-number id="total_revenue" v-model.number="form['total_revenue']"
						v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','total_revenue')) || (!form['business_analysis_id'] && $check_field('add','total_revenue'))" :disabled="disabledObj['total_revenue_isDisabled']"></el-input-number>
					<div v-else-if="$check_field('get','total_revenue')">{{form['total_revenue']}}</div>
							</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','remarks') || $check_field('add','remarks') || $check_field('set','remarks')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="备注" prop="remarks">
												<el-input id="remarks" v-model="form['remarks']" placeholder="请输入备注"
							  v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','remarks')) || (!form['business_analysis_id'] && $check_field('add','remarks'))" :disabled="disabledObj['remarks_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','remarks')">{{form['remarks']}}</div>
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
				field: "business_analysis_id",
				url_add: "~/api/business_analysis/add?",
				url_set: "~/api/business_analysis/set?",
				url_get_obj: "~/api/business_analysis/get_obj?",
				url_upload: "~/api/business_analysis/upload?",

				query: {
					"business_analysis_id": 0,
				},

				form: {
								"title":  '', // 标题
										"seller_no": 0, // 商家编号
										"store_name":  '', // 店铺名称
										"statistical_time":  '', // 统计时间
										"total_revenue":  0, // 收入总额
										"remarks":  '', // 备注
											"business_analysis_id": 0, // ID
						
				},
				disabledObj:{
								"title_isDisabled": false,
										"seller_no_isDisabled": false,
										"store_name_isDisabled": false,
										"statistical_time_isDisabled": false,
					          			"total_revenue_isDisabled": false,
										"remarks_isDisabled": false,
										},

	
		
					// 用户列表
				list_user_seller_no: [],
						// 用户组
				group_user_seller_no: "",
				
		
		
		
	
			}
		},
		methods: {


	
	
			
	
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
					bl = this.$check_action('/business_analysis/table','add');
					console.log(bl ? "你有表格添加权限视作有添加权限" : "你没有表格添加权限");
				}
				if(!bl){
					bl = this.$check_action('/business_analysis/table','set');
					console.log(bl ? "你有表格添加权限视作有修改权限" : "你没有表格修改权限");
				}
				if(!bl){
					bl = this.$check_action('/business_analysis/view','add');
					console.log(bl ? "你有视图添加权限视作有添加权限" : "你没有视图添加权限");
				}
				if(!bl){
					bl = this.$check_action('/business_analysis/view','set');
					console.log(bl ? "你有视图修改权限视作有修改权限" : "你没有视图修改权限");
				}
				if(!bl){
					bl = this.$check_action('/business_analysis/view','get');
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
