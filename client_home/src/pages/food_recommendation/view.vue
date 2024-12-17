<template>
  <view>
    <view class="container diy_view">
      <view>
        <view>
          <view class="">
            <uni-forms :modelValue="form"  v-if="is_view()">

              <uni-forms-item v-if="$check_field('get','product_name') || ($check_field('add','product_name') || $check_field('set','product_name'))" label="餐品名称" name="product_name">
                            <uni-easyinput type="text" v-model="form['product_name']" v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','product_name')) || (!form['food_recommendation_id'] && $check_field('add','product_name'))" :disabled="disabledObj['product_name_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','product_name')">
                  {{ form['product_name'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','picture') || ($check_field('add','picture') || $check_field('set','picture'))" label="图片" name="picture">
                    <!-- 修改权限 -->
                <view class="diy_field diy_img" v-if="form['picture'] && $check_field('set','picture')">
                  <image v-if="disabledObj['picture_isDisabled']" :src="$fullUrl(form['picture'])" />
                  <image v-if="!disabledObj['picture_isDisabled']" :src="$fullUrl(form['picture'])" @click="change_img('picture')" />
                </view>
                <!-- 添加权限 -->
                <view class="diy_field diy_img" v-else-if="!form['picture'] && $check_field('add','picture')">
                  <view v-if="disabledObj['picture_isDisabled']" class="btn_add_img">
                    <text>+</text>
                  </view>
                  <view v-if="!disabledObj['picture_isDisabled']" class="btn_add_img" @click="change_img('picture')">
                    <text>+</text>
                  </view>
                </view>
                <!-- 查询权限 -->
                <view class="diy_field diy_img" v-else-if="$check_field('get','picture')">
                  <image :src="$fullUrl(form['picture'])" />
                </view>
                  </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','type') || ($check_field('add','type') || $check_field('set','type'))" label="类型" name="type">
                            <uni-easyinput type="text" v-model="form['type']" v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','type')) || (!form['food_recommendation_id'] && $check_field('add','type'))" :disabled="disabledObj['type_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','type')">
                  {{ form['type'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','flavor') || ($check_field('add','flavor') || $check_field('set','flavor'))" label="口味" name="flavor">
                            <uni-easyinput type="text" v-model="form['flavor']" v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','flavor')) || (!form['food_recommendation_id'] && $check_field('add','flavor'))" :disabled="disabledObj['flavor_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','flavor')">
                  {{ form['flavor'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','raw_material') || ($check_field('add','raw_material') || $check_field('set','raw_material'))" label="原材料" name="raw_material">
                            <uni-easyinput type="text" v-model="form['raw_material']" v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','raw_material')) || (!form['food_recommendation_id'] && $check_field('add','raw_material'))" :disabled="disabledObj['raw_material_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','raw_material')">
                  {{ form['raw_material'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','seller_no') || ($check_field('add','seller_no') || $check_field('set','seller_no'))" label="商家编号" name="seller_no">
                    <uni-data-select
                  id="form_seller_no"
                  v-model="form['seller_no']"
                  :localdata="list_user_seller_no"
                  :clear="!disabledObj['seller_no_isDisabled']"
                  :disabled="disabledObj['seller_no_isDisabled']"
                  v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','seller_no')) || (!form['food_recommendation_id'] && $check_field('add','seller_no'))"
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
                            <uni-easyinput type="text" v-model="form['store_name']" v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','store_name')) || (!form['food_recommendation_id'] && $check_field('add','store_name'))" :disabled="disabledObj['store_name_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','store_name')">
                  {{ form['store_name'] }}
                </text>
                          </uni-forms-item>
              <uni-forms-item v-if="$check_field('get','detailed_introduction') || ($check_field('add','detailed_introduction') || $check_field('set','detailed_introduction'))" label="详情介绍" name="detailed_introduction">
                    <uni-easyinput type="textarea" v-model="form['detailed_introduction']" v-if="user_group === '管理员' || (form['food_recommendation_id'] && $check_field('set','detailed_introduction')) || (!form['food_recommendation_id'] && $check_field('add','detailed_introduction'))" :disabled="disabledObj['detailed_introduction_isDisabled']" />
                <!-- 仅查看 -->
                <text v-else-if="$check_field('get','detailed_introduction')">
                  {{ form['detailed_introduction'] }}
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
            url: _self.$fullUrl('/api/food_recommendation/upload?'),
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
            url: _self.$fullUrl('/api/food_recommendation/upload?'),
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
