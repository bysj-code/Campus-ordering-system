<template>
  <view>
    <view class="container diy_view">
      <view>
        <view>
          <view class="">
            <uni-forms :modelValue="form"  v-if="is_view()">

              <uni-forms-item v-if="$check_field('get','title') || ($check_field('add','title') || $check_field('set','title'))" label="标题" name="title">
                            <uni-easyinput type="text" v-model="form['title']" v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','title')) || (!form['business_analysis_id'] && $check_field('add','title'))" :disabled="disabledObj['title_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','title')">
                  {{ form['title'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','seller_no') || ($check_field('add','seller_no') || $check_field('set','seller_no'))" label="商家编号" name="seller_no">
                    <uni-data-select
                  id="form_seller_no"
                  v-model="form['seller_no']"
                  :localdata="list_user_seller_no"
                  :clear="!disabledObj['seller_no_isDisabled']"
                  :disabled="disabledObj['seller_no_isDisabled']"
                  v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','seller_no')) || (!form['business_analysis_id'] && $check_field('add','seller_no'))"
                ></uni-data-select>
                <uni-data-select
                  v-model="form['seller_no']"
                  :localdata="list_user_seller_no"
                  :clear="false"
                  :disabled="true"
                  v-else-if="$check_field('get','seller_no')" id="seller_no"
                ></uni-data-select>
                  </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','store_name') || ($check_field('add','store_name') || $check_field('set','store_name'))" label="店铺名称" name="store_name">
                            <uni-easyinput type="text" v-model="form['store_name']" v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','store_name')) || (!form['business_analysis_id'] && $check_field('add','store_name'))" :disabled="disabledObj['store_name_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','store_name')">
                  {{ form['store_name'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','statistical_time') || ($check_field('add','statistical_time') || $check_field('set','statistical_time'))" label="统计时间" name="statistical_time">
                            <uni-easyinput type="text" v-model="form['statistical_time']" v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','statistical_time')) || (!form['business_analysis_id'] && $check_field('add','statistical_time'))" :disabled="disabledObj['statistical_time_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','statistical_time')">
                  {{ form['statistical_time'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','total_revenue') || ($check_field('add','total_revenue') || $check_field('set','total_revenue'))" label="收入总额" name="total_revenue">
                            <uni-easyinput type="text" v-model="form['total_revenue']" v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','total_revenue')) || (!form['business_analysis_id'] && $check_field('add','total_revenue'))" :disabled="disabledObj['total_revenue_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','total_revenue')">
                  {{ form['total_revenue'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','remarks') || ($check_field('add','remarks') || $check_field('set','remarks'))" label="备注" name="remarks">
                            <uni-easyinput type="text" v-model="form['remarks']" v-if="user_group === '管理员' || (form['business_analysis_id'] && $check_field('set','remarks')) || (!form['business_analysis_id'] && $check_field('add','remarks'))" :disabled="disabledObj['remarks_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','remarks')">
                  {{ form['remarks'] }}
                </text>
                          </uni-forms-item>


            </uni-forms>
            <view class="form_button">
              <button size="mini" type="primary" @click="submit()" class="primary_btn">提交</button>
              <button size="mini" @click="cancel()">取消</button>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import mixin from "@/libs/mixins/page.js";

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
                    "total_revenue":  0 , // 收入总额
                    "remarks":  '', // 备注
                                "business_analysis_id": 0, // ID
                
              },
          disabledObj:{
                        "title_isDisabled": false,
                                "seller_no_isDisabled": false,
                                "store_name_isDisabled": false,
                                "statistical_time_isDisabled": false,
                                                    "remarks_isDisabled": false,
                                  },
                                                    // 用户列表
            list_user_seller_no: [],
                        // 用户组
            group_user_seller_no: "",
                                                                                                                          }
  },
  methods: {
    changeLog(v,value){
      this.form[value] = v
    },
    /**
     * 上传文件
     * @param {Object} param文件参数
     */
    change_file(key_name){
      var _self = this;
      // 选择图像方法
      uni.chooseFile({
        count: 1,
        sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
        sourceType: ['album'], //从相册选择
        success: function(res) {
          const tempFilePaths = res.tempFilePaths;
          const uploadTask = uni.uploadFile({
            url: _self.$fullUrl('/api/business_analysis/upload?'),
            filePath: tempFilePaths[0],
            name: 'file',
            formData: {
              'i_want_to_customize': 'test'
            },
            header: {
              'x-auth-token': _self.$store.state.user.token
            },
            success: function(uploadFileRes) {
              var filename = JSON.parse(uploadFileRes.data).result.url
              _self.form[key_name] = filename
            }
          });

          uploadTask.onProgressUpdate(function(res) {
            _self.percent = res.progress;
            console.log('上传进度' + res.progress);
            console.log('已经上传的数据长度' + res.totalBytesSent);
            console.log('预期需要上传的数据总长度' + res.totalBytesExpectedToSend);
          });
        },
        error: function(e) {
          console.log(e);
        }
      });
    },
    /**
     * 上传图片
     * @param {Object} param文件参数
     */
    change_img(key_name){
      var _self = this;
      _self.upload_img_flag = false
      // 选择图像方法
      uni.chooseImage({
        count: 1,
        sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
        sourceType: ['album'], //从相册选择
        success: function(res) {
          const tempFilePaths = res.tempFilePaths;
          const uploadTask = uni.uploadFile({
            url: _self.$fullUrl('/api/business_analysis/upload?'),
            filePath: tempFilePaths[0],
            name: 'file',
            formData: {
              'i_want_to_customize': 'test'
            },
            header: {
              'x-auth-token': _self.$store.state.user.token
            },
            success: function(uploadFileRes) {
              var filename = JSON.parse(uploadFileRes.data).result.url
              _self.form[key_name] = filename
            }
          });

          uploadTask.onProgressUpdate(function(res) {
            _self.percent = res.progress;
            console.log('上传进度' + res.progress);
            console.log('已经上传的数据长度' + res.totalBytesSent);
            console.log('预期需要上传的数据总长度' + res.totalBytesExpectedToSend);
          });
        },
        error: function(e) {
          console.log(e);
        }
      });
    },
    /**
     * 获取对象后获取缓存表单
     * @param {Object} json
     * @param {Object} func
     */
    get_obj_before(param){
      var form = uni.db.get("form");
      if (form) {
        delete(form.examine_state)
        delete(form.examine_reply)
        this.obj = uni.push(this.obj ,form);
        this.form = uni.push(this.form ,form);
      }
      var arr = []
      for (let key in form) {
        arr.push(key)
      }
      for (var i=0;i<arr.length;i++){
        this.disabledObj[arr[i] + '_isDisabled'] = true
      }
                                                      uni.db.del("form");
      return param;
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
        json.result.list.map((o) => this.list_user_seller_no.push({value:o.user_id,text:o.nickname + '-' + o.username}));
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
        this.get_user_session_seller_no(this.form['seller_no'])
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
            
            
            
            
            
    
    /**
     * 获取对象之后
     * @param {Object} json
     * @param {Object} func
     */
    get_obj_after(json, func){
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

  },
  created() {
                    this.get_list_user_seller_no();
            this.get_group_user_seller_no();
                                              },
}
</script>

<style scoped>
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

.form_button{
  padding-bottom: 15px;
  display: flex;
}
.form_button button{
  width: 40%;
}
.query_select{
  border-color: rgb(229, 229, 229);
  background-color: rgb(255, 255, 255);
  border-radius: 4px;
  box-sizing: border-box;
  flex: 1;
  width: 100%;
  line-height: 2;
  font-size: 14px;
  height: 2.4em;
  min-height: 2.4em;
  display: block;
  outline:none;
}

.query_option{
  width: 100%;
}

.btn_add_img{
  color: #D3D3D3;
  text-align: center;
  border: 1px solid #eee;
  height: 5rem;
  width: 5rem;
  position: relative;
}
.btn_add_img text{
  font-size: 35px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}

.form_button {
  padding-bottom: 15px;
  display: flex;
}
.form_button button {
  width: 40%;
}
.query_select {
  border-color: rgb(229, 229, 229);
  background-color: rgb(255, 255, 255);
  border-radius: 4px;
  box-sizing: border-box;
  flex: 1;
  width: 100%;
  line-height: 2;
  font-size: 14px;
  height: 2.4em;
  min-height: 2.4em;
  display: block;
  outline: none;
}

.query_option {
  width: 100%;
}

.btn_add_img {
  color: #d3d3d3;
  text-align: center;
  border: 1px solid #eee;
  height: 5rem;
  width: 5rem;
  position: relative;
}
.btn_add_img text {
  font-size: 35px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
/*新样式*/
.uni-forms{
padding-top:1rem;
}
.uni-forms-item {
	padding: 6px 10px;
    background: #f8f6fc;
}
.uni-forms .is-input-border{
	border: 0;
}
.container{
	    -webkit-box-shadow: 0px 0px 0px #888888;
	    box-shadow: 0px 0px 0px #888888;
}
.form_button .primary_btn{
		background-color: #22B8B8;
		color: #FFFFFF;
	}
</style>
