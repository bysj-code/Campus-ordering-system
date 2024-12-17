<template>
	<el-main class="bg edit_wrap">
		<el-form ref="form" :model="form" status-icon label-width="120px" v-if="is_view()">

							<el-col v-if="user_group === '管理员' || $check_field('get','user_number') || $check_field('add','user_number') || $check_field('set','user_number')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="用户编号" prop="user_number">
																		<div v-if="user_group !== '管理员'">
							{{ get_user_session_user_number(form['user_number']) }}
							<!--<el-input id="business_name" v-model="form['user_number']" placeholder="请输入用户编号"-->
							<!--v-if="user_group === '管理员' || (form['my_friend_id'] && $check_field('set','user_number')) || (!form['my_friend_id'] && $check_field('add','user_number'))" :disabled="disabledObj['user_number_isDisabled']"></el-input>-->
							<!--<div v-else-if="$check_field('get','user_number')">{{form['user_number']}}</div>-->
							<el-select v-if="user_group === '管理员' || (form['my_friend_id'] && $check_field('set','user_number')) || (!form['my_friend_id'] && $check_field('add','user_number'))" id="user_number" v-model="form['user_number']" :disabled="disabledObj['user_number_isDisabled']">
								<el-option v-for="o in list_user_user_number" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
							<el-select v-else-if="$check_field('get','user_number')" id="user_number" v-model="form['user_number']" :disabled="true">
								<el-option v-for="o in list_user_user_number" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
						</div>
						<el-select v-else id="user_number" v-model="form['user_number']" :disabled="disabledObj['user_number_isDisabled']">
							<el-option v-for="o in list_user_user_number" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
									   :value="o['user_id']">
							</el-option>
						</el-select>
																</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','friend_no') || $check_field('add','friend_no') || $check_field('set','friend_no')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="好友编号" prop="friend_no">
													<el-select v-if="user_group === '管理员' || (form['my_friend_id'] && $check_field('set','friend_no')) || (!form['my_friend_id'] && $check_field('add','friend_no'))" id="friend_no" v-model="form['friend_no']" :disabled="disabledObj['friend_no_isDisabled']">
							<el-option v-for="o in list_user_friend_no" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
									   :value="o['user_id']">
							</el-option>
						</el-select>
						<el-select v-else-if="$check_field('get','friend_no')" id="friend_no" v-model="form['friend_no']" :disabled="true">
							<el-option v-for="o in list_user_friend_no" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
									   :value="o['user_id']">
							</el-option>
						</el-select>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','friend_name') || $check_field('add','friend_name') || $check_field('set','friend_name')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="好友名称" prop="friend_name">
												<el-input id="friend_name" v-model="form['friend_name']" placeholder="请输入好友名称"
							  v-if="user_group === '管理员' || (form['my_friend_id'] && $check_field('set','friend_name')) || (!form['my_friend_id'] && $check_field('add','friend_name'))" :disabled="disabledObj['friend_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','friend_name')">{{form['friend_name']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','friend_type') || $check_field('add','friend_type') || $check_field('set','friend_type')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="好友类型" prop="friend_type">
												<el-input id="friend_type" v-model="form['friend_type']" placeholder="请输入好友类型"
							  v-if="user_group === '管理员' || (form['my_friend_id'] && $check_field('set','friend_type')) || (!form['my_friend_id'] && $check_field('add','friend_type'))" :disabled="disabledObj['friend_type_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','friend_type')">{{form['friend_type']}}</div>
											</el-form-item>
			</el-col>
								<el-col v-if="user_group === '管理员' || $check_field('get','remarks') || $check_field('add','remarks') || $check_field('set','remarks')" :xs="24" :sm="12" :lg="8" class="el_form_item_warp">
				<el-form-item label="备注" prop="remarks">
												<el-input id="remarks" v-model="form['remarks']" placeholder="请输入备注"
							  v-if="user_group === '管理员' || (form['my_friend_id'] && $check_field('set','remarks')) || (!form['my_friend_id'] && $check_field('add','remarks'))" :disabled="disabledObj['remarks_isDisabled']"></el-input>
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
				field: "my_friend_id",
				url_add: "~/api/my_friend/add?",
				url_set: "~/api/my_friend/set?",
				url_get_obj: "~/api/my_friend/get_obj?",
				url_upload: "~/api/my_friend/upload?",

				query: {
					"my_friend_id": 0,
				},

				form: {
								"user_number": 0, // 用户编号
										"friend_no": 0, // 好友编号
										"friend_name":  '', // 好友名称
										"friend_type":  '', // 好友类型
										"remarks":  '', // 备注
											"my_friend_id": 0, // ID
						
				},
				disabledObj:{
								"user_number_isDisabled": false,
										"friend_no_isDisabled": false,
										"friend_name_isDisabled": false,
										"friend_type_isDisabled": false,
										"remarks_isDisabled": false,
										},

	
					// 用户列表
				list_user_user_number: [],
						// 用户组
				group_user_user_number: "",
				
					// 用户列表
				list_user_friend_no: [],
				
		
		
	
			}
		},
		methods: {


	
	
				/**
			 * 获取注册用户用户列表
			 */
			async get_list_user_user_number() {
                // if(this.user_group !== "管理员" && this.form["user_number"] === 0) {
                //     this.form["user_number"] = this.user.user_id;
                // }
                var json = await this.$get("~/api/user/get_list?user_group=注册用户");
                if(json.result && json.result.list){
                    this.list_user_user_number = json.result.list;
                }
                else if(json.error){
                    console.error(json.error);
                }
			},
					/**
			 * 获取注册用户用户组
			 */
			async get_group_user_user_number() {
							this.form["user_number"] = this.user.user_id;
							var json = await this.$get("~/api/user_group/get_obj?name=注册用户");
				if(json.result && json.result.obj){
					this.group_user_user_number = json.result.obj;
				}
				else if(json.error){
					console.error(json.error);
				}
			},
			get_user_session_user_number(id){
				var _this = this;
				var user_id = {"user_id":id}
				var url = "~/api/"+_this.group_user_user_number.source_table+"/get_obj?"
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
			
									_this.form["user_number"] = id
									_this.disabledObj['user_number' + '_isDisabled'] = true
						for (var i=0;i<arr.length;i++){
						  if (arr[i]!=='examine_state' && arr[i]!=='examine_reply') {
							for (var j = 0; j < arrForm.length; j++) {
							  if (arr[i] === arrForm[j]) {
								if (arr[i] !== "user_number") {
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
					get_user_user_number(id){
				var obj = this.list_user_user_number.getObj({"user_id":id});
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
			 * 获取注册用户用户列表
			 */
			async get_list_user_friend_no() {
                // if(this.user_group !== "管理员" && this.form["friend_no"] === 0) {
                //     this.form["friend_no"] = this.user.user_id;
                // }
                var json = await this.$get("~/api/user/get_list?user_group=注册用户");
                if(json.result && json.result.list){
                    this.list_user_friend_no = json.result.list;
                }
                else if(json.error){
                    console.error(json.error);
                }
			},
					get_user_friend_no(id){
				var obj = this.list_user_friend_no.getObj({"user_id":id});
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
					bl = this.$check_action('/my_friend/table','add');
					console.log(bl ? "你有表格添加权限视作有添加权限" : "你没有表格添加权限");
				}
				if(!bl){
					bl = this.$check_action('/my_friend/table','set');
					console.log(bl ? "你有表格添加权限视作有修改权限" : "你没有表格修改权限");
				}
				if(!bl){
					bl = this.$check_action('/my_friend/view','add');
					console.log(bl ? "你有视图添加权限视作有添加权限" : "你没有视图添加权限");
				}
				if(!bl){
					bl = this.$check_action('/my_friend/view','set');
					console.log(bl ? "你有视图修改权限视作有修改权限" : "你没有视图修改权限");
				}
				if(!bl){
					bl = this.$check_action('/my_friend/view','get');
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
					this.get_list_user_user_number();
					this.get_group_user_user_number();
								this.get_list_user_friend_no();
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
