
<template>
  <el-container>
    <el-header>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>导览展示</el-breadcrumb-item>
      </el-breadcrumb>
    </el-header>
    <el-container>
      <el-aside width="65%" style="border: 1px #000 solid">
        <div id="main" style="width: 95%;height: 700px"  ref="myEchart"></div>
      </el-aside>
      <el-main>
        <p>{{detail.name}}</p>
        <img :src="detail.picture" class="detail"></img>
        <p>{{detail.info}}</p>
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
import echarts from 'echarts'
export default{
  name:'show',
  data(){
    return{
      chart: {},
      desc: '',
      detail: {},
      all: {},
      mapping: {
        '科室1': 1,
        '科室2': 2
      },
      options: {
        series: [
          {
            type: 'treemap',
            data: [
              {
                name: '前台',            // First tree
                value: 10,
                children: [
                  {
                      name: '前台',       // First leaf of first tree
                      value: 6
                  }, 
                  {
                      name: '排号机',       // Second leaf of first tree
                      value: 4
                  }
                ]
              },
              {
                name: '科室1',            // First tree
                value: 10,
                children: [
                  {
                      name: '科室1',       // First leaf of first tree
                      value: 8
                  }, 
                  {
                      name: '科室2',       // Second leaf of first tree
                      value: 2
                  }
                ]
              },
              {
                name: '门诊',            // First tree
                value: 30,
                children: [
                  {
                      name: '内科门诊',       // First leaf of first tree
                      value: 10,
                      children: [
                        {
                          name: '呼吸内科',
                          value: 5
                          
                        },
                        {
                          name: '消化内科',
                          value: 5
                        },
                        {
                          name: '心血管内科',
                          value: 5
                        },
                        {
                          name: '神经内科',
                          value: 5
                        },
                        {
                          name: '肿瘤科',
                          value: 5
                        },
                        {
                          name: '内分泌科',
                          value: 5
                        },
                        {
                          name: '血液内科',
                          value: 5
                        },
                        {
                          name: '传染病科',
                          value: 5
                        }
                      ]
                  }, 
                  {
                      name: '外科门诊',       // First leaf of first tree
                      value: 10,
                      children: [
                        {
                          name: '外科大夫',
                          value: 5
                        },
                        {
                          name: '外科大夫助理',
                          value: 5
                        }
                      ]
                  }, 
                  {
                      name: '皮肤科门诊',       // First leaf of first tree
                      value: 10,
                      children: [
                        {
                          name: '皮肤科大夫',
                          value: 5
                        },
                        {
                          name: '皮肤科大夫助理',
                          value: 5
                        }
                      ]
                  }, 
                ]
              }, 
              {
                name: '手术室',            // Second tree
                value: 30,
                children: [
                  {
                    name: '科室1',       // Son of first tree
                    value: 10,
                    children: [
                      {
                        name: '科室1',  // Granson of first tree
                        value: 50
                      },
                      {
                        name: '科室1',  // Granson of first tree
                        value: 20
                      }
                    ]
                  },
                  {
                    name: '科室2',       // Son of first tree
                    value: 10,
                    children: [
                      {
                        name: '科室2',  // Granson of first tree
                        value: 50
                      },
                      {
                        name: '科室2',  // Granson of first tree
                        value: 20
                      }
                    ]
                  },
                  {
                    name: '2号手术室',       // Son of first tree
                    value: 10,
                    children: [
                      {
                        name: '2号手术室主室',  // Granson of first tree
                        value: 50
                      },
                      {
                        name: '2号手术室器材室',  // Granson of first tree
                        value: 20
                      }
                    ]
                  }
                ]
              }
            ]
          }
        ],
        tooltip: {
          formatter: function (info) {
              var value = info.name;
              var treePathInfo = info.treePathInfo;
              var treePath = [];

              for (var i = 1; i < treePathInfo.length; i++) {
                  treePath.push(treePathInfo[i].name);
              }

              return [
                  '<div class="tooltip-title">' + value + '</div>',
                  ,
              ].join('');
          }
        }

      }
    }
  },
  computed: {

  },
  mounted () {
    let vm = this
    this.chart = echarts.init(this.$refs.myEchart);
    this.chart.setOption(this.options)
    this.chart.on('click', function (params) {

      vm.desc = params.data.name
      vm.detail = vm.all[vm.mapping[vm.desc]] || {name: '尚未有信息',picture:'',info:'请找锦华添加数据'}
    });
    vm.$http.get(
      '/pet/facility/getAll'
    ).then(function(res) {
      vm.all = res.data

    })
  },
  methods: {

  }
}
</script>
<style scoped>
.moxing {
  width: 760px;
  height: 580px;
}
.detail {
  width: 80%;
  height: auto;
}
</style>