<template>
	<div class="candidateAdd">
		<h2>添加候选人</h2>
		<div class="mess">
			
			<el-form :inline="true" :model="form" class="demo-form-inline"  :rules="rules" ref="form" 
				method="post" enctype ="multipart/form-data">
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				<el-form-item label="姓名" prop="candidateName">
				    <el-input v-model="form.candidateName" clearable></el-input>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="签约类型">
				    <el-select v-model="candidateAcceptSignType" @change="selectSignType" placeholder="请选择" clearable>
				      	<el-option
                  			v-for="item in signTypeList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>
				    </el-select>
				   
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="工作年限" prop="candidateWorkingSeniority">
				    <el-select v-model="candidateWorkingSeniority" @change="selectWorkingSeniority">
				      	<el-option
                  			v-for="item in workingSeniorityList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>
				    </el-select>
				</el-form-item><br />
				
				<el-form-item label="期望薪水" prop="candidateCostMin">				  
				    <el-input v-model="form.candidateCostMin" style = "width:60px"></el-input clearable> K/月 &nbsp;~&nbsp;
				    <el-input v-model="form.candidateCostMax" style = "width:60px" clearable></el-input> K/月				  
				</el-form-item>
				<el-form-item label="预计到岗时间" prop="candidateOnboardTime">
				    <el-date-picker 
				    	v-model="form.candidateOnboardTime"
				    	type="date"
      					placeholder="预计到岗时间" value-format='yyyy/MM/dd HH:mm:ss' @change="changeTime">
   					</el-date-picker>
				</el-form-item><br />
				
				<el-form-item label="毕业院校" prop="candidateGraduateSchoolName">
				    <el-input v-model="form.candidateGraduateSchoolName" placeholder="请输入" clearable></el-input>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="学历名称" prop="candidateQualificationName">
				    <el-select v-model="candidateQualificationName" @change="selectQualificationName">
				      	<el-option
                  			v-for="item in qualificationNameList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>
				    </el-select>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="毕业时间" prop="candidateGraduateDay">  
   					<el-date-picker
					    v-model="form.candidateGraduateDay"
					    type="date"
					    placeholder="毕业时间">
    				</el-date-picker>
				</el-form-item><br />&nbsp;&nbsp;
				
				<el-form-item label="居住地" prop="selectedOptions">
			<!--		<el-input v-model="form.home" clearable></el-input>-->
				    	<el-cascader
		                		size="large"
		                		:options="options"
		                		v-model="form.selectedOptions"
		                		clearable>
		              	</el-cascader>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="证件类型" prop="请选择">
				    <el-select v-model="candidateIdTypeName" @change="selectIdTypeName">
				      	<el-option
                  			v-for="item in IdTypeNameList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>
				    </el-select>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="证件号" prop="candidateIdNo">
				    <el-input v-model="form.candidateIdNo" clearable></el-input>
				</el-form-item><br />&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="电话" prop="candidateTelNo">
				    <el-input v-model="form.candidateTelNo" clearable></el-input>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
								&nbsp;&nbsp;
				
				<el-form-item label="邮箱" prop="candidateEmail">
				    <el-input v-model="form.candidateEmail" clearable></el-input>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
								&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
								
				<el-form-item label="微信" prop="candidateWx">
				    <el-input v-model="form.candidateWx" clearable></el-input>
				</el-form-item><br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

				<el-form-item label="QQ" prop="candidateQq">
				    <el-input v-model="form.candidateQq" clearable></el-input>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="紧急提醒" prop="candidateWarnName">
				    <el-select v-model="candidateWarnName" @change="selectWarnName">
				      		<el-option
                  			v-for="item in warnNameList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>
				    </el-select>
				</el-form-item><br />
				
				<el-form-item label="附件简历">
					 <el-upload
						  ref="upload"
						  :action="fileUrl"
						  :file-list="fileList"
						  method='post'
						  :data="form.formData"
						  :headers="{Authorization:token, username:username}"
						  accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel,.csv,text/plain"
						  :auto-upload="false">
						  <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
						  <!--<el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">保存</el-button>-->
						</el-upload>
				</el-form-item><br />

				<el-form-item label="技能类别" prop="candidateSkillType">
				    <el-select v-model="skillFirst.candidateSkillType" @change="selectSkillType">
				    	<el-option
                  			v-for="item in skillTypeList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>	   	
				    </el-select>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="技能年限" prop="candidateSkillYear">
				    <el-select v-model="skillFirst.candidateSkillYear" @change="selectSkillYear">
				      	<el-option
                  			v-for="item in workingSeniorityList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>
				    </el-select>
				</el-form-item><el-button type="primary" icon="el-icon-plus" @click="addSkill"></el-button><br />
				
				<el-form-item label="技能类别" prop="candidateSkillTypeSecond" v-if="show">
				    <el-select v-model="skillSecond.candidateSkillTypeSecond" @change="selectSkillTypeSecond">
				    	<el-option
                  			v-for="item in skillTypeList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>	   	
				    </el-select>
				</el-form-item>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				
				<el-form-item label="技能年限" prop="candidateSkillYearSecond" v-if="show">
				    <el-select v-model="skillSecond.candidateSkillYearSecond" @change="selectSkillYearSecond">
				      	<el-option
                  			v-for="item in workingSeniorityList"
                  			:key="item.id"
                  			:label="item.itemValue"
                  			:value="item.itemCode">
                		</el-option>
				    </el-select>
				</el-form-item><el-button type="primary" icon="el-icon-minus" v-if="show" @click="minuSkill"></el-button><br />
				<div class="footer">
					<el-button type="primary" @click="saveCandidate">保存</el-button>
					<el-button @click="handleBack">取消</el-button>
				</div>
	
			</el-form>
		</div>
		
	</div>
</template>

<script>

	import { candidateAdd,getList } from '@/api/pages/mainPage/candidateManagement';
  	import api from '@/axios/api.js';
  	import { provinceAndCityData,CodeToText, TextToCode } from 'element-china-area-data';
  	import { mapGetters } from 'vuex';
	import '@/assets/styles/reset.scss';
	 
	export default {

    	data() {
      		return {
      			
      			show:false,
      			candidateQualificationName:'',
      			candidateAcceptSignType : '',
      			title:this.$route.query.title,
      			candidateWorkingSeniority : '',
      			candidateIdTypeName:'',
      			candidateWarnName:'',
      			candidateSkill:[
      				
      			],
      			skillFirst:{
      				candidateSkillType:'',
      				candidateSkillYear:'',
      			},
      			skillSecond:{
      				candidateSkillTypeSecond:'',
      				candidateSkillYearSecond:'', 
      			},
      			    			
      			fileUrl:api.api +'/project/candidateInfo/insertCandidate',
        		fileList:[],
  
        		option: {
		          target: 'http://10.100.100.232:8080/project/candidateInfo/insertCandidate',
		          testChunks: false
		        },
		         attrs: {
		          accept: 'image/*'
		        },
		        signTypeList:[], //签约类型列表
        		workingSeniorityList:[], //工作年限列表
        		qualificationNameList:[], //学历名称列表
        		options:provinceAndCityData, //省市联动数据
        		IdTypeNameList:[], //证件类型列表
        		warnNameList:[], //紧急提醒列表
        		skillTypeList:[],//掌握技能
				
        		form: {
        			
        			formData:{}, 
	          		candidateName: '',          		//候选人名字
	          		candidateAcceptSignType: {
	          			itemValue:'',
	          			itemCode:'1'
	          		},		//合同类型
	          		candidateWorkingSeniority:{
	          			itemCode:'1'
	          		},		//工作年限
	          		candidateCostMin:'',				//期望工资
	          		candidateCostMax:'',
	          		candidateOnboardTime:'',			//预计到岗时间
	          		candidateGraduateSchoolName:'',		//毕业院校
	          		candidateQualificationName:{
	          			itemCode:'1'
	          		}, 		//学历
	          		candidateGraduateDay:'',			//毕业时间
	          		options:provinceAndCityData,
		        	selectedOptions:[],
//		        	home:'',							//住址
//					homeEmpty:'',
	          		candidateIdTypeName:{
	          			itemCode:'1'
	          		},				//证件类型
	          		candidateIdNo:'',					//证件号
	          		candidateTelNo:'',					//电话
	          		candidateEmail:'',					//邮箱
	          		candidateWx:'',						//微信
	          		candidateQq:'',						//QQ
	          		candidateWarnName:{
	          			itemCode:'1'
	          		},				//紧急提醒
	          		candidateSkillType:{
	          			itemCode:'1'
	          		},					//熟悉技能
	          		candidateSkillTypeSecond:{
	          			itemCode:'1',
	          		},
	          		candidateSkillYear:{
	          			itemCode:'1'
	          		},				//技能掌握时间
	          		candidateSkillYearSecond:{
	          			itemCode:'1'
	          		},
	          		
        		},
        		rules:{
        			
        			candidateName:[
			            {required:true,message:'请输入候选人姓名',trigger:'blur'}
			        ],
			        candidateAcceptSignType:[
			            {required:true,message:'请选择签约类型',trigger:'change'}
			        ],
			        candidateCostMin:[
			            {required:true,message:'请选择期望薪水下限',trigger:'blur'}
			        ],
			        candidateCostMax:[
			            {required:true,message:'请选择期望薪水上限',trigger:'blur'}
			        ],
			        candidateOnboardTime:[
			            {required:true,message:'请选择预计到岗时间',trigger:'change'}
			        ],
			        candidateGraduateSchoolName:[
			            {required:true,message:'请输入毕业学校',trigger:'blur'}
			        ],
			        candidateQualificationName:[
			            {required:true,message:'请选择学历名称',trigger:'change'}
			        ],
			        candidateGraduateDay:[
			            {required:true,message:'请选择毕业时间',trigger:'change'}
			        ],
			        candidateIdTypeName:[
			            {required:true,message:'请选择证件类型',trigger:'change'}
			        ],
			        candidateIdNo:[
			            {required:true,message:'请输入证件号码',trigger:'blur'}	
			        ],
			        candidateTelNo:[
			            {required:true,message:'请输入手机号码',trigger:'blur'},
						{pattern: /^(13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])\d{8}$/, message: '手机号格式不正确', trigger: 'blur'}
			        ],
			        candidateEmail:[
						{required: true, message: '请输入邮箱地址', trigger: 'blur'},
						{type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change']}
					],
					candidateIdSkillType:[
			            {required:true,message:'请选择技能类型',trigger:'change'}
			        ],
			        candidateIdSkillYear:[
			            {required:true,message:'请选择技能年限',trigger:'change'}
			        ],
        		}
      		}
    	},
    	computed: {
			...mapGetters([
				'token', 'username'
			])
		},
		mounted(){
	      	this.setNewsApi();
	      	this.dataJudge();
	    },
		
    	methods: {
    		
    		//时间格式
		    changeTime(time){
	            this.candidateOnboardTime = time;
	        },
    		
      		//获取所属列表
	      	setNewsApi() {
	          	getList(['project.signType','req.candidate.skill.duration','project.candidate.qualification','project.candidate.IDType','project.candidate.warn','req.candidate.skill.type'],
	          	{Authorization:this.token, username:this.username}).then((res) => {
	              	console.log(res)
	              	this.signTypeList = res.body.data["project.signType"].sysDictItemList; //获取签约类型列表
	              	this.workingSeniorityList = res.body.data["req.candidate.skill.duration"].sysDictItemList; //获取工作年限列表和技能技能年限市一条数据
	              	this.qualificationNameList = res.body.data["project.candidate.qualification"].sysDictItemList; //获取学历名称列表
	              	this.IdTypeNameList = res.body.data["project.candidate.IDType"].sysDictItemList; //获取证件类型列表
	              	this.warnNameList = res.body.data["project.candidate.warn"].sysDictItemList; //获取紧急提醒列表
	              	this.skillTypeList = res.body.data["req.candidate.skill.type"].sysDictItemList;//获取技能类别列表
	          	});
	      	},
	      	
	      	//获取签约类型的id和name
	      	selectSignType(vId){//这个vId也就是value值
	        		let obj  = {};
	        		obj = this.signTypeList.find((item)=>{ //这里的signTypeList就是上面遍历的数据源
	            		return item.itemCode === vId; //筛选出匹配数据
	        		});
	        		this.form.candidateAcceptSignType = obj.itemCode;
	        		this.candidateAcceptSignType = this.form.candidateAcceptSignType;
	      	},
	      	//获取工作年限的id和name
	      	selectWorkingSeniority(vId){ //这个vId也就是value值
	        		let obj  = {};
	        		obj = this.workingSeniorityList.find((item)=>{ //这里的workingSeniorityList就是上面遍历的数据源
	            		return item.itemCode === vId; //筛选出匹配数据
	        		});
	        		this.form.candidateWorkingSeniority = obj.itemCode;
	        		this.candidateWorkingSeniority = this.form.candidateWorkingSeniority;
	      	},
	      	//获取学历名称的id和name
	      	selectQualificationName(vId){ //这个vId也就是value值
	        		let obj  = {};
	        		obj = this.qualificationNameList.find((item)=>{ //这里的qualificationNameList就是上面遍历的数据源
	            		return item.itemCode === vId; //筛选出匹配数据
	        		});
	        		this.form.candidateQualificationName = obj.itemCode;
	        		this.candidateQualificationName = this.form.candidateQualificationName;
	      	},
	      	//获取证件类型的id和name
	      	selectIdTypeName(vId){ //这个vId也就是value值
	        		let obj  = {};
	        		obj = this.IdTypeNameList.find((item)=>{ //这里的IdTypeNameList就是上面遍历的数据源
	            		return item.itemCode === vId; //筛选出匹配数据
	        		});
	        		this.form.candidateIdTypeName = obj.itemCode;
	        		this.candidateIdTypeName = this.form.candidateIdTypeName;
	      	},
	      	//获取紧急提醒的id和name
	      	selectWarnName(vId){ //这个vId也就是value值
	        		let obj  = {};
	        		obj = this.warnNameList.find((item)=>{ //这里的warnNameList就是上面遍历的数据源
	            		return item.itemCode === vId; //筛选出匹配数据
	        		});
	        		this.form.candidateWarnName = obj.itemCode;
	        		this.candidateWarnName = this.form.candidateWarnName;
	      	},
	      	//获取技能类别的id和name
	      	selectSkillType(vId){ //这个vId也就是value值
	        		let obj  = {};
	        		obj = this.skillTypeList.find((item)=>{ //这里的skillTypeList就是上面遍历的数据源
	            		return item.itemCode === vId; //筛选出匹配数据
	        		});
	        		this.form.candidateSkillType = obj.itemValue;
	        		this.skillFirst.candidateSkillType = this.form.candidateSkillType;
	      	},
	      	selectSkillTypeSecond(vId){ //这个vId也就是value值
	        		let obj  = {};
	        		obj = this.skillTypeList.find((item)=>{ //这里的skillTypeList就是上面遍历的数据源
	            		return item.itemCode === vId; //筛选出匹配数据
	        		});
	        		this.form.candidateSkillTypeSecond = obj.itemValue;
	        		this.skillSecond.candidateSkillTypeSecond = this.form.candidateSkillTypeSecond;
	      	},
	      	//获取技能年限的id和name
	      	selectSkillYear(vId){ //这个vId也就是value值
	      			let obj = {};
	      			obj = this.workingSeniorityList.find((item)=>{ //这里的workingSeniorityList就是上面遍历的数据源
	      				return item.itemCode === vId; //筛选出匹配数据
	      			});
	      			this.form.candidateSkillYear = obj.itemValue;
	      			this.skillFirst.candidateSkillYear = this.form.candidateSkillYear;
	      	},
	      	selectSkillYearSecond(vId){ //这个vId也就是value值
	      			let obj = {};
	      			obj = this.workingSeniorityList.find((item)=>{ //这里的workingSeniorityList就是上面遍历的数据源
	      				return item.itemCode === vId; //筛选出匹配数据
	      			});
	      			this.form.candidateSkillYearSecond = obj.itemValue;
	      			this.skillSecond.candidateSkillYearSecond = this.form.candidateSkillYearSecond;
	      	},
	      	
		   submitUpload() {
				this.form.formData={
					candidateName : this.form.candidateName,
					candidateAcceptSigntypeCode :this.candidateAcceptSignType,
					candidateWorkingSeniority : this.candidateWorkingSeniority,
					candidateCostMax : this.form.candidateCostMax,
					candidateCostMin : this.form.candidateCostMin,
					candidateOnboardTime : this.form.candidateOnboardTime,
					candidateGraduateSchoolName : this.form.candidateGraduateSchoolName,
					candidateQualificationCode : this.candidateQualificationName,
					candidateGraduateDay : this.form.candidateGraduateDay,
					candidateLocaitonProvince : this.form.selectedOptions[0],
					candidateLocaitonCity : this.form.selectedOptions[1],
					candidateIdTypeCode : this.candidateIdTypeName,
					candidateIdNo : this.form.candidateIdNo,
					candidateTel : this.form.candidateTelNo,
					candidateEmail : this.form.candidateEmail,
					candidateWx : this.form.candidateWx,
					candidateQq : this.form.candidateQq,
					candidateWarnCode : this.candidateWarnName,
					candidateSkill:this.candidateSkill,
				}
		        setTimeout(() => { 
					this.$refs.upload.submit();	
//					this.$router.push({path:'candidate'})
				}, 1000);
			
		      },
		    //保存
      		saveCandidate(formName){
      			this.$refs[formName].validate((valid) => {
			        if (valid) {
			            alert('submit!');
			        } else {
			            console.log('error submit!!');
			            return false;
			        }
			    });
      			this.candidateSkill.push(this.skillFirst.candidateSkillType,this.skillFirst.candidateSkillYear);
      				
      			this.candidateSkill.push(this.skillSecond.candidateSkillTypeSecond,this.skillSecond.candidateSkillYearSecond);
 //     				  				this.$router.push({path:'candidate'})
					this.submitUpload();
  			},
  			handleBack(){
  				this.$router.back();
  			},
  			
  			addSkill(){
  				this.show = true;
  				alert("123");
  			},
  			minuSkill(){
  				this.show = false;
  			},
      

	        	dataJudge(){
		        	if(this.$route.query.candidateData){
		          	let {
		          		candidateName,
						candidateAcceptSignType,
						candidateWorkingSeniority,
						candidateCostMax,
						candidateCostMin,
						candidateOnboardTime,
						candidateGraduateSchoolName,
						candidateQualificationName,
						candidateGraduateDay,
						candidateLocaitonProvince,
						candidateLocaitonCity,
						candidateIdTypeName,
						candidateIdNo,
						candidateTel,
						candidateEmail,
						candidateWx,
						candidateQq,
						candidateWarnName,
						skillType,
						skillDuration
		          	} = this.$route.query.candidateData;
		        		this.form = {
		        		candidateName,
						candidateAcceptSignType,
						candidateWorkingSeniority,
						candidateCostMax,
						candidateCostMin,
						candidateOnboardTime,
						candidateGraduateSchoolName,
						candidateQualificationName,
						candidateGraduateDay,
						candidateLocaitonProvince,
						candidateLocaitonCity,
						candidateIdTypeName,
						candidateIdNo,
						candidateTel,
						candidateEmail,
						candidateWx,
						candidateQq,
						candidateWarnName,
						skillType,
						skillDuration
		        		};
			        this.ruleForm.selectedOptions=[];
			        this.ruleForm.selectedOptions.push(this.ruleForm.candidateLocaitonCity);
		        }
		    }
	         
    	},
    	
  }
	
</script>

<style scoped lang="scss">
	.candidateAdd{
		margin: 0px,auto;
  		background-image:none;
		position:static;
		left:auto;
		width:1123px;
		margin-left:0;
		margin-right:0;
		text-align:left;
		
		h2{
			margin-left: 20px;
			float: left;
		}
		.mess{
			margin-top: 50px;
			margin-left: 100px;
			float: left;
			
			span{
				display: inline-block;
				font-size: small;
			}
		}
		.footer{
				margin-top:10px;
				text-align: center;
			}
		
	}
</style>