<template>
	<div>
			<el-form-item v-if="$check_register_field('add','seller_no','/seller/view')" label="商家编号" prop="seller_no">
				<el-input id="seller_no" v-model="form['seller_no']" placeholder="请输入商家编号"></el-input>
				</el-form-item>
				<el-form-item v-if="$check_register_field('add','store_name','/seller/view')" label="店铺名称" prop="store_name">
				<el-input id="store_name" v-model="form['store_name']" placeholder="请输入店铺名称"></el-input>
				</el-form-item>
				<el-form-item v-if="$check_register_field('add','person_in_charge','/seller/view')" label="负责人" prop="person_in_charge">
				<el-input id="person_in_charge" v-model="form['person_in_charge']" placeholder="请输入负责人"></el-input>
				</el-form-item>
	
	</div>
</template>

<script>
	import mixin from "@/mixins/component.js";

	export default {
		mixins: [mixin],
		props:{

			form:{
				type: Object,
				default: function(){
					return {
							"seller_no":  '' ,
								"store_name":  '' ,
								"person_in_charge":  '' ,
						}
				}
			},

		},
		data() {
			return {
				field: "seller_id",
				url_add: "~/api/seller/add?",
				url_set: "~/api/seller/set?",
				url_upload: "~/api/seller/upload?",

			
				rules: {
					"seller_no": [    {required: true,message: '商家编号不能为空'},  ],
					"store_name": [    {required: true,message: '店铺名称不能为空'},  ],
					"person_in_charge": [    {required: true,message: '负责人不能为空'},  ],
				}

			}
		},
		methods: {

	
	
	

	
	
	

	
	
	

		},
		created() {
					}
	}
</script>
