//index.vue
<template>
  <div class="bg">
    <img :src="bgpic" class="bgpic"></img>
    <div :class="{ 'bgtxtMobile': isMobile, 'bgtxt': notMobile}">
      <div :class="{ 'singleMobile': isMobile, 'single': notMobile}" v-for="(item, index) in carou">
        <div style=" font-family: 'PingFang SC';color: #026660;font-size: 26px">{{item.title}}</div>
        <div>{{item.desc}}</div>
        <el-button type="primary" @click="open(item.open)">{{item.btnText}}</el-button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'home',
  data () {
    return {
       carou: [
        {
          title: '导览展示',
          desc: '以3D模型分科室详尽的展示宠物医院',
          bg: '', // 图片
          open: '2', // 点击后打开的页面
          btnText: '去体验' //按钮上的文字
        },
        {
          title: '角色扮演',
          desc: '扮演前台、医助、医师,了解各个职位',
          bg: '', // 图片
          open: '3-1', // 点击后打开的页面
          btnText: '去扮演' //按钮上的文字
        },
        {
          title: '病例学习',
          desc: '这里有最丰富最全面的宠物病例',
          bg: '', // 图片
          open: '3-2', // 点击后打开的页面
          btnText: '去学习' //按钮上的文字
        },
        {
          title: '测试答题',
          desc: '对您的学习情况进行阶段性的测试',
          bg: '', // 图片
          open: '4', // 点击后打开的页面
          btnText: '去测试' //按钮上的文字
        }
       ]
    }
  },
  methods:{
    open (key) {
      switch(key){
        case '2':
          this.$router.push('/show')
          break
        case '3-1':
          if (!this.$store.state.user.name) {
            this.$alert('只有登录用户才可以体验角色扮演哦！', '提示', {
              confirmButtonText: '去登陆',
              callback: action => {
                this.$router.push('/login')
              }
            });
          }
          else {
            this.$router.push('/play')
          }
          break
        case '3-2':
          if (!this.$store.state.user.name) {
            this.$alert('只有登录用户才可以学习病例哦！', '提示', {
              confirmButtonText: '去登陆',
              callback: action => {
                this.$router.push('/login')
              }
            });
          }
          else {
            this.$router.push('/learn')
          }
          break
        case '4':
          if (!this.$store.state.user.name) {
            this.$alert('只有登录用户才可以进行测试哦！', '提示', {
              confirmButtonText: '去登陆',
              callback: action => {
                this.$router.push('/login')
              }
            });
          }
          else {
            this.$router.push('/test')
          }
          break
        default:
          this.$router.push('/')
      }
    }
  },
  computed: {
    isMobile () {
      return document.body.clientWidth < 500
    },
    notMobile () {
      return document.body.clientWidth >= 500
    },
    bgpic () {
      return this.isMobile ? '../src/assets/imgs/back2.jpg' : '../src/assets/imgs/back.jpg'
    }
  }
}
</script>

<style scoped>
@import url("//unpkg.com/element-ui@2.2.2/lib/theme-chalk/index.css");
.bg {
  position: relative;
  width: 95%;
  padding-left: 2.5%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: visible;
}
.bgpic {
  width: 100%;
  height: auto;
}
.bgtxtMobile{
  width: 100%;
  height: auto;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}
.bgtxt{
  width: 100%;
  height: auto;
  flex-grow: 1;
  margin-top: -20px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}
.singleMobile{
  width: auto;
  height: auto;
  background: #fff;
  display: flex;
  margin-top: 15px;
  padding: 10px 15px 10px 15px;
  justify-content: space-around;
  align-items: center;
  background: #fff;
}
.single{
  width: auto;
  height: 200px;
  background: #fff;
  border-top: 5px solid #026660;
  display: flex;
  margin: 0 20px 0 20px;
  padding: 0 20px 0 20px;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}
.el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }
/*  .el-carousel__item{
    border-bottom: 1px #fff solid;
  }*/ 
  .el-carousel__item:nth-child(2n) {
    background-color: #17B794;
    color: #fff;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #17B794;
    color: #fff;
  }
.singleCarousel{
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  -webkit-display: flex;
  flex-direction: column;
  -webkit-flex-direction: column;
  align-items: center;
  justify-content: center;
}
.singleCarousel div{
  text-align: center;
}
.singleCarousel .title, .singleCarousel .desc{
  font-size: 40px;
  height: 60px;
  line-height: 60px;
}
.singleCarousel .desc{
  font-size: 28px;
  width: 100%;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  margin-top: 35px;
}
.singleCarousel .btn{
  margin-top: 100px;
  padding: 6px 30px 6px 30px;
  height: 60px;
  text-align: center;
  line-height: 60px;
  border: 1px #fff solid;
  font-size: 30px;
  border-radius: 5px;
}
.singleCarousel .btn:hover{
  color: #000;
  background: #fff;
}
</style>

