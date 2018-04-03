
<template>
  <el-container>
  	<el-header>
  		<el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: '/learn' }">病例学习主页</el-breadcrumb-item>
        <el-breadcrumb-item>病例xxx</el-breadcrumb-item>
      </el-breadcrumb>
  	</el-header>
  	<el-container>
      <el-header style="text-align: center;">
        {{data.text || '文字部分'}}
      </el-header>
      <el-container class="detail">
        <video width="80%" height="350" controls>
          <source :src="data.videoUrl" type="video/mp4">
          <source :src="data.videoUrl" type="video/ogg">
          <source :src="data.videoUrl" type="video/webm">
        您的浏览器不支持 video 标签。
        </video>

        <el-carousel :interval="2000" type="card" height="200px" style="width: 80%;margin-top: 15px">
          <el-carousel-item v-for="(picture,index) in data.picUrl" :key="index">
            <img :src="picture" class="pic"></img>
          </el-carousel-item>
        </el-carousel>
      </el-container>
  	</el-container>
  </el-container>
</template>
<script>
export default{
  name: 'case',
  data () {
    return{
      caseID: null,
      data: {}
    }
  },
  created() {
    let vm = this
    vm.caseID = this.$route.params.case
    vm.$http.get(
      '/pet/case/getCaseDetail?caseID=' + vm.caseID
    ).then(function(res) {
      vm.datas = res.data
    })
  },
  methods: {

  }
}
</script>
<style scoped>
.zujian {
	width: 50%;
	height: 500px;
	display: flex;
	flex-direction: column;
	align-items: flex-start;
}
.btn{
	align-self: flex-end;
	margin-top: 30px;
}
.pic{
  width: 100%;
  height: auto;
}
.detail {
  margin-left: 25px;
  background: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #fff;
}
</style>