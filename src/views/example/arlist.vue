<template>
  <div class="app-container">
    <el-container style="height: 800px; border: 1px solid #eee">
        <el-aside width="300px" style="background-color: rgb(238, 241, 246);" >
        <el-date-picker
          v-model="pldate"
          type="date"
          placeholder="选择日期"
          class="pldate">
        </el-date-picker>
        <el-select v-model="country" placeholder="请选择" class="sleop">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-aside>
      <el-container>
        
        <el-main>
          <el-table v-loading="listLoading" :data="list" border fit highlight-current-row style="width: 100%;">
            <el-table-column align="center" label="标题" style="width: 10%;">
              <template slot-scope="scope">
                <router-link :to="'/example/arlistdetail/'+scope.row.md5" class="link-type" style="width: 10%;">
                  <span>{{ scope.row.title }}</span>
                </router-link>
              </template>
            </el-table-column>
            <el-table-column style="width: 10%;" align="center" label="摘要">
              <template slot-scope="scope">
                <span>{{ scope.row.abstract }}</span>
              </template>
            </el-table-column>
            <el-table-column style="width: 10%;" align="center" label="来源">
              <template slot-scope="scope">
                <span>{{ scope.row.source }}</span>
              </template>
            </el-table-column>
            <el-table-column style="width: 10%;" align="center" label="URL">
              <template slot-scope="scope">
                <span>{{ scope.row.url }}</span>
              </template>
            </el-table-column>
            <el-table-column style="width: 10%;" align="center" label="时间">
              <template slot-scope="scope">
                <span>{{ scope.row.time | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
              </template>
            </el-table-column>
          </el-table>
        </el-main>
        <el-footer>
          <pagination 
            v-show="total>10" 
            :total="total" 
            :page.sync="listQuery.page" 
            :limit.sync="listQuery.limit" 
            @pagination="getList" 
            class= "pageclass"
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
             layout="total, sizes, prev, pager, next, jumper"/>
              <!-- <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="currentPage4"
                :page-sizes="[10, 20, 30, 50]"
                :page-size="100"
                layout="total, sizes, prev, pager, next, jumper"
                :total="400">
              </el-pagination> -->
        </el-footer>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import { fetchList,sddddArticle } from '@/api/article'
import Pagination from '@/components/Pagination' // Secondary package based on el-pagination

export default {
  name: 'ArList',
  components: { Pagination },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      total: 0,
      listLoading: true,
      currentPage4:1,
      listQuery: {
        page: 1,
        limit: 20
      },
       pldate:'',
       options: [{
          value: 'yd',
          label: '印度'
        }, {
          value: 'yn',
          label: '越南'
        }, {
          value: 'oz',
          label: '澳洲'
        }, {
          value: 'mg',
          label: '美国'
        }],
        country: '',
    }
  },
  created() {
    this.getList()
  },
  methods: {
    handleSizeChange(val) {
      this.listQuery.limit = val
      console.log("111111")
      this.getList()
    },
    handleCurrentChange(val) {
      console.log("22222")
      this.listQuery.page = val
      this.getList()
    },
    getList() {
      // this.list = null;
      this.listLoading = true
      console.log("this.listQuery.page===="+this.listQuery.page)
      console.log("this.listQuery.limit=----"+this.listQuery.limit)
      sddddArticle({page:this.listQuery.page,page_size:this.listQuery.limit}).then(response =>{
          this.list=response.data.results
          this.total=response.data.count
          this.listLoading=false
          console.log(response)
      })
    }
  }
}
</script>

<style scoped>
.edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;
}
.el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
  .pageclass{
    padding: 6px 16px;
    margin-top: 8px;
  }
  .el-aside-select{
    margin-top: 30px;
  }
  .sleop{
    margin-top: 10px;
    width: 94%;
  }
</style>
