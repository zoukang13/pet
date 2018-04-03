<template>
   <el-container>
    <el-header>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item >病例学习主页</el-breadcrumb-item>
      </el-breadcrumb>
    </el-header>
    <el-container>
    	<el-header>
    		<span class="demonstration">请选择病例种类</span>
				<el-cascader
				expand-trigger="hover"
				:options="options"
				v-model="selectedOptions2"
				@change="handleChange">
				</el-cascader>
    	</el-header>
    	<el-main>
    		<el-table :data="tableData">
		      <el-table-column
		        prop="name"
		        label="名"
		        min-width="150">
		      </el-table-column>
		      <el-table-column
		        prop="date"
		        label="日期"
		        min-width="150">
		      </el-table-column>
		      <el-table-column
		        prop="pet"
		        label="宠物名"
		        min-width="180">
		      </el-table-column>
		      <el-table-column
		        prop="old"
		        label="年龄"
		        min-width="120">
		      </el-table-column>
		      <el-table-column
		        prop="desc"
		        label="描述"
		        min-width="180">
		      </el-table-column>
		      <el-table-column
		        prop="status"
		        label="治疗状态"
		        min-width="180">
		      </el-table-column>
		      <el-table-column
			      label="操作"
			      min-width="100">
			      <template slot-scope="scope">
			        <el-button  type="primary" size="small" @click="detail(scope.row.id)">查看</el-button>
			      </template>
			    </el-table-column>
		    </el-table>

        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="page.page"
          :page-sizes="[5, 10, 20, 50]"
          :page-size="page.size"
          layout="total, sizes, prev, pager, next, jumper"
          :total="page.total">
        </el-pagination>
    	</el-main>
    </el-container>
  </el-container>
</template>
<script>
export default{
  name:'learn',
  data(){
    return{
      options: [
      ],
      page: {
        page: 1,
        size: 5,
        total: 0
      },
      selectedOptions: [],
      selectedOptions2: [],
      tableData: [
      ],
      diseaseID: 0
    }
  },
  computed: {
    query () {
      return `/pet/case/getCase?diseaseID=3&page=${this.page.page}&size=${this.page.size}`
    }
  },
  created () {
    let vm = this
    this.$http.get(
        '/pet/case/getallDisease'
      ).then(function(res) {
      vm.options = res.data.map((item, index) => {
        item.value = index
        item.label = item.lable
        item.children.forEach((i, j) => {
          i.value = i.id
          i.label = i.name
        })
        return item
      })

    }).catch(function(err) {})
  },
  methods: {
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.page.size = val
    },
    handleCurrentChange(val) {
      this.page.page = val
      console.log(`当前页: ${val}`);
    },
  	handleChange (value) {
      this.diseaseID = value[1]
  	},
  	detail (id) {
  		this.$router.push({name: 'case', params: { case: id}})
  	}
  },
  watch: {
    query (newval, oldval) {
      let vm = this
      vm.$http.get(
        newval
      ).then(function(res) {
        vm.tableData = res.data.caseInfoList
        vm.page.total  =res.data.count
      })
    }
  }
}
</script>
<style scoped>
.el-pagination{
  padding: 12px 0;
  display: flex;
  justify-content: flex-end;
}
</style>