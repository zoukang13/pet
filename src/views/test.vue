
<template>
  <el-container>
  	<el-header>
  		<el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>测试</el-breadcrumb-item>
      </el-breadcrumb>
  	</el-header>
  	<el-container>
  		<el-aside :width="isMobile? '25%' : '18%'">
  			<el-menu
		      default-active="1"
		      class="el-menu-vertical-demo"
		      @select="handleOpen"
		      @close="handleClose"
		      background-color="#fff">
		    	<el-menu-item index="1">
		        <i class="el-icon-menu"></i>
		        <span slot="title">测试</span>
		      </el-menu-item>
		      <el-menu-item index="2">
		        <i class="el-icon-menu"></i>
		        <span slot="title">答题记录</span>
		      </el-menu-item>
		    </el-menu>
  		</el-aside>
  		<el-main style="padding: 25px;background: #fff;margin-left: 20px;">
  			<div class="zujian" v-if="prepare && showTest && !hasscore">
          <p>请组建您的测试卷</p>
	  			<el-transfer
				    v-model="chosenCat"
				    filterable
				    :titles="['题目分类', '目标考卷']"
				    :button-texts="['到题库', '到考卷']"
				    :format="{
				      noChecked: '${total}',
				      hasChecked: '${checked}/${total}'
				    }"
				    @change="handleChange"
				    :data="data">
				  </el-transfer>
          <div class="btns">
            <div>
              <span style="line-height: 40px">考题数量</span>
              <el-input v-model.number="num" class="ipt" placeholded="考题数量" ></el-input>
            </div>
            <div style="height: 40px">(题目数量可能不足)</div>
            <el-button type="primary" class="btn" @click="goTest">去考试</el-button>
          </div>  
				</div>

				<div v-if="!prepare && showTest && !hasscore">
					<el-card class="box-card">
					  <div slot="header" class="clearfix">
					    <span>试卷</span>
					    <el-button style="float: right; padding: 3px 0" type="text" @click="reprepare">重新组卷</el-button>
					  </div>
					  <div v-for="(o, i) in questionList" :key="i" class="text item">
					    <p>{{i}}. {{o.stem}}</p>
              <el-radio v-model="o.choice" :label="o['a']" border style="margin-left: 10px">{{o['a']}}</el-radio>
              <el-radio v-model="o.choice" :label="o['b']" border>{{o['b']}}</el-radio>
              <el-radio v-model="o.choice" :label="o['c']" border>{{o['c']}}</el-radio>
              <el-radio v-model="o.choice" :label="o['d']" border>{{o['d']}}</el-radio>
					  </div>
            <div class="submit">
              <el-button type="primary" @click="submit">去交卷</el-button>
            </div>
					</el-card>
				</div>

        <div v-if="hasscore && showTest" v-loading="loading1"
          element-loading-text="提交考卷中"
          element-loading-spinner="el-icon-loading">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>试卷得分</span>
              <el-button style="float: right; padding: 3px 0" type="text" @click="reprepare">重新组卷</el-button>
            </div>
            <div  class="text item">
             <p>您的本次得分情况为 {{score}} 分(一道题10分哦)</p>
            </div>

          </el-card>
        </div>

				<div v-if="!showTest">
					<el-card class="box-card">
					  <div slot="header" class="clearfix">
					    <span>历史答题记录</span>
					  </div>
					  <div v-for="(o, i) in historyList" :key="i" class="text item">
					    <p>{{i}}. {{o.stem}} <i :class="o.TorF == 'T' ? 'el-icon-check' : 'el-icon-close'" style="color:red"></i></p>
              <el-radio v-model="o.choice" :label="o['A']" border disabled style="margin-left: 10px">{{o['A']}}</el-radio>
              <el-radio v-model="o.choice" :label="o['B']" border disabled>{{o['B']}}</el-radio>
              <el-radio v-model="o.choice" :label="o['C']" border disabled>{{o['C']}}</el-radio>
              <el-radio v-model="o.choice" :label="o['D']" border disabled>{{o['D']}}</el-radio>
					  </div>
					</el-card>
				</div>
  		</el-main>
  	</el-container>
  </el-container>
</template>
<script>
export default{
  name: 'test',
  data () {
    return{
      data: [],
      chosenCat: [],
      prepare: true,
      questionList: [],
      key: '1',
      num: 10,
      hasscore: false,
      score: 0,
      loading1: false,
      historyList: []
    }
  },
  computed: {
		showTest () {
			return this.key == '1' ? true : false
		},
    isMobile () {
      return document.body.clientWidth < 500
    },
    notMobile () {
      return document.body.clientWidth >= 500
    }
  },
  created() {
  	let vm = this
    this.$http.get(
      '/pet/case/getAllCategory'
    ).then(function(res) {
    vm.data = res.data.map(item => {
      item.key = item.category
      item.label = item.name
      return item
    })
    }).catch(function(err) {})
  },
  methods: {
  	handleOpen(key, keyPath) {

      this.key = key
    },
    handleClose(key, keyPath) {

    },
    handleChange () {

    },
    goTest () {
      let vm = this
      console.log(this.chosenCat)
    	if (this.chosenCat.length == 0) {
    		this.$notify({
          title: '提示',
          message: '请先选择考试题的种类哦！'
        });
        return false
    	}
      if (isNaN(this.num) || (this.num < 1)) {
        this.$notify({
          title: '提示',
          message: '请输入正确的考题数量哦！'
        });
        return false
      }
      if (this.num > 25) {
        this.$notify({
          title: '提示',
          message: '你可以少做点题吗!'
        });
        this.num = 25
        return false
      }
  		this.prepare = false

      this.$http.post(
        '/pet/test/questions',
        {
          CategoryList: vm.chosenCat,
          num: vm.num
        },
        {
          'Access-Control-Allow-Origin': '*'
        }
      ).then(function(res) {
        
        vm.questionList = res.data.questionList
        if (res.data.questionList.length > 0)
          vm.prepare = false
        else {
          vm.prepare = true
          vm.$notify({
            title: '提示',
            message: '当前分类下没有题目！'
          });
        }
      }).catch(function(err) {
        console.log(err)
      })
    },
    reprepare () {
    	this.prepare = true
      this.hasscore = false
      this.score = null
    	this.questionList = []
    },
    submit () {
      let vm = this
      let submitData = this.questionList.map(item => {
        let a = {}
        a.ques_id = item.id
        a.choose_ans = item.choice || ''
        return a
      })
      vm.loading1 = true
      vm.hasscore = true
      this.$http.post(
        '/pet/test/getScore',
        {
          select: submitData,
          user_id: parseInt(vm.$store.state.user.id)
        },
        {
          'Access-Control-Allow-Origin': '*'
        }
      ).then(function(res) {
        vm.loading1 = false
        vm.score = res.data
        console.log(res)
      }).catch(function(err) {
        console.log(err)
      })
    }
  },
  watch: {
    showTest () {
      let vm = this
      this.$http.get(
        '/pet/test/getRecord?user_id=' + vm.$store.state.user.id
      ).then(function(res) {
        vm.historyList = res.data
        console.log(vm.historyList)
      }).catch(function(err) {})
    }
  }
}
</script>

<style scoped>
@import url("../assets/styles/ele.css");

.zujian {
	width: 100%;
	height: 500px;
	display: flex;
	flex-direction: column;
	align-items: flex-start;
}
.btns {
  width: 100%;
  margin-top: 0px;
  height: auto;
  display: flex;
  flex-direction: column;
}
.btns div{
  width: 100%;
  margin-top: 10px;
}
.btns .ipt{
  width: 80px;
  align-self: flex-start;
  margin-left: 15px;
}
.btn{
  margin-top: 5px;
  width: 50%;
}
.submit {
  display: flex;
  width: 100%;
  margin-top: 20px;
  flex-direction: row-reverse;
}
.el-radio.is-bordered.is-disabled {
  border-color: #409EFF;

}
.el-menu-item {
  padding: 0;
  padding-left: 0 !important;
  text-align: center;
}
.el-radio__input.is-disabled + span .el-radio__label{
  color: #409EFF !important;
}
.el-radio__input.is-disabled .el-radio__inner, .el-radio__input.is-disabled.is-checked .el-radio__inner{
  background-color: #409EFF;
}
</style>