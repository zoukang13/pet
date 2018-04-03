
<template>
  <el-container>
    <el-header>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: '/play' }">角色扮演主页</el-breadcrumb-item>
        <el-breadcrumb-item>特定角色扮演</el-breadcrumb-item>
      </el-breadcrumb>
    </el-header>
    <el-container>
      <el-aside width="35%" class="process">
        <p>流程</p>
        <div class="singleProcess" v-for="(item, index) in arr">
          <p>{{item.label}}</p>
          <el-steps :active="item.active" finish-status="success">
            <el-step v-for="(i,j) in item.children" :title="i.step_name"  @click.native="detail(i.id)"></el-step>
  <!--           <el-step title="步骤 1" description="这是一段很长很长很长的描述性文字"></el-step>
            <el-step title="步骤 2" description="这是一段很长很长很长的描述性文字"></el-step>
            <el-step title="步骤 3" description="这是一段很长很长很长的描述性文字"></el-step> -->
          </el-steps>

          <el-button type="success" @click="next(index)">下一步</el-button>
        </div>
      </el-aside>
      <el-main class="detail">
        <p>流程详情</p>
        <video width="80%" height="350" controls>
          <source :src="details.videoUrl" type="video/mp4">
          <source :src="details.videoUrl" type="video/ogg">
          <source :src="details.videoUrl" type="video/webm">
        您的浏览器不支持 video 标签。
        </video>
        <el-carousel :interval="2000" type="card" height="200px" style="width: 80%;margin-top: 15px">
          <el-carousel-item v-for="(picture,index) in details.picUrl" :key="index">
            <img :src="picture" class="pic"></img>
          </el-carousel-item>
        </el-carousel>
        <p>{{details.info}}</p>
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
import echarts from 'echarts'
export default{
  name:'show',
  data(){
    return {
      msg: '',
      active: 0,
      arr: [],
      id: null,
      details: {}
    }
  },
  created() {
    this.msg = '这里是' + this.$route.params.role + '扮演页'
    let vm = this
    this.$http.get(
      '/pet/rolePlay/getRoleProcedure?role=1'
    ).then(function(res) {
      console.log(res.data)
      vm.arr = res.data.map(item => {
        item.active = 0
        return item
      })
    }).catch(function(err) {})
  },
  mounted () {
    
  },
  methods: {
    next(index) {
      if (this.arr[index].active++ > (this.arr[index].children.length -1)) this.arr[index].active = 0;
      if (this.arr[index].active > 0) {
        this.id = this.arr[index].children[this.arr[index].active - 1].id
      }
      //console.log(this.arr[index].children[this.arr[index].active].id)
    },
    detail(id) {
      this.id = id
    }
  },
  watch: {
    id (newval, oldval) {
      let vm = this
      this.$http.get(
        '/pet/rolePlay/getProcedureDetail?procedureID=' + newval
      ).then(function(res) {
        vm.details = res.data
        
      }).catch(function(err) {})
    }
  }
}
</script>
<style scoped>
.moxing {
  width: 760px;
  height: 580px;
}
.process{
  display: flex;
  flex-direction: column;
  align-items: center;

}
.singleProcess{
  width: 90%;
  padding: 20px 15px 20px 15px;
}
.detail {
  margin-left: 25px;
  background: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #fff;
}
.pic{
  width: 100%;
  height: auto;
}
</style>