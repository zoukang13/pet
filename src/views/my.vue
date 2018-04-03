
<template>
  <el-container>
  	<el-header>
  		<el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>我的</el-breadcrumb-item>
      </el-breadcrumb>
  	</el-header>
  	<el-container>
  		<el-aside>
  			<img :src="imageUrl" class="imgs" alt="头像">
  			<el-button type="primary" @click="logout">退出登录</el-button>
  		</el-aside>

  	</el-container>
  </el-container>
</template>
<script>
import utils from '../util.js'
export default{
  name:'login',
  data(){
    return{
    }
  },
  computed: {
  	imageUrl () {
  		if (this.$store.state.user.picture_url) {
  			return 'https://www.ecnupet.cn:8080/pet/' + this.$store.state.user.picture_url
  		}
  		else
  			return ''
  	}
  },
  created () {
  	console.log(this.$store.state.user)
  },
  methods: {
  	logout () {
  		this.$store.commit("login", {name:'',picture_url:'',id:null});
    	utils.delCookie('username')
    	utils.delCookie('picture_url')
    	utils.delCookie('userid')
  	 this.$notify({
        title: '提示',
        message: '退出成功'
      });
  	}
  }
}
</script>
<style scoped>
.login-bottom {
	width: 100%;
	margin-top: 30px;
	display: flex;
	flex-direction: row-reverse;
}
.imgs {
	width: 150px;
	height: auto;

}
</style>