<template>
  <body>
  <h1>发布新帖子</h1>
  <el-form ref="form" :model="form" label-width="80px" :rules="rules">
    <el-form-item label="帖子标题" prop="title">
      <el-input v-model="form.title" ></el-input>
    </el-form-item>
    <!--进行列表渲染，数据全部存在Vuex的全局静态变量里，修改分区数只需要修改store/index.js的静态数据-->
    <el-form-item label="活动分类" prop="category">
      <el-select  filterable v-model="form.category" placeholder="请选择分类">
        <el-option
          v-for="item in this.$store.state.homepageClass"
          :key="item.home"
          :value="item.typeId"
          :label="item.home"
        >
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="帖子内容" prop="content">
      <el-input type="textarea" v-model="form.content" :rows="20"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit">提交</el-button>
      <el-button @click="returnMain">取消</el-button>
    </el-form-item>
  </el-form>

  </body>

</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      postsId:'',
      form: {
        title: '',
        category: '',
        content: ''
      },
      rules:{
        title:[{required : true,message: '请输入标题',trigger:'blur'}],
        content:[{required : true,message: '请输入内容',trigger:'blur'}],
        category:[{required : true,message: '请选择分类',trigger:'change'}],
      }
    }
  },
  created() {

    if(this.$store.state.localid===''||this.$store.state.localid===undefined){
      alert("未登录，无法发布")
      this.$router.push('/login')
    }
  },
  methods: {
    onSubmit() {
      //alert('submit!');
      if(this.form.content===''||this.form.title===''||this.form.category==="")
      {
        alert("有未填写项，无法发布")
      }
      else if(this.$store.state.localid===''||this.$store.state.localid===undefined){
        alert("未登录，无法发布")
      }
      else {
        const self = this;
        self.$axios({
          method:'post',
          url:'/post',
          data:{
            category:self.form.category,
            title:self.form.title,
            content:self.form.content
          }
        })
        .then(res=>{
          if(res.data.data.flag===true)
          {
           alert(res.data.message)
            console.log(res)
            self.postsId=res.data.postsId
          }
          else {
           // alert(res.data.message)
            alert(res.data.message)
            console.log(res)
          }

        })
      }
    },
    returnMain(){

      this.$router.push("homepageone?typeId=1&page=1");
      //测试路由跳转
      //this.$router.push({ name: 'homepageone', params: { typeId: '1' ,page:'1'} })
    }

  }
}
</script>

<style scoped>

</style>
