<template>
  <div class="app-container" style="background-color:#e5e9f2;">
    <el-row :gutter="20">
      <el-col :span="2"><div class="grid-content bg-purple box1">md5：</div></el-col>
      <el-col :span="10"><div class="grid-content bg-purple box2">{{ this.list.md5}}</div></el-col>
      <el-col :span="2"><div class="grid-content bg-purple box1">新闻时间：</div></el-col>
      <el-col :span="10"><div class="grid-content bg-purple box2">{{ this.list.time}}</div></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="2"><div class="grid-content bg-purple box1" >URL：</div></el-col>
      <el-col :span="10"><div class="grid-content bg-purple box2">{{ this.list.url}}</div></el-col>
      <el-col :span="2"><div class="grid-content bg-purple box1">网站名称：</div></el-col>
      <el-col :span="10"><div class="grid-content bg-purple box2">{{ this.list.website}}</div></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="2"><div class="grid-content bg-purple box1">标题：</div></el-col>
      <el-col :span="11"><div class="grid-content bg-purple box2">{{ this.list.title}}</div></el-col>
      <el-col :span="11"><div class="grid-content bg-purple box2">{{ this.list.title_translated}}</div></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="2"><div class="grid-content bg-purple box1">摘要：</div></el-col>
      <el-col :span="11"><div class="grid-content bg-purple box2">{{ this.list.abstract }}</div></el-col>
      <el-col :span="11"><div class="grid-content bg-purple box2">{{ this.list.abstract_translated}}</div></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="2"><div class="grid-content bg-purple box1">内容：</div></el-col>
      <el-col :span="11"><div class="grid-content bg-purple box2">{{ this.list.content}}</div></el-col>
      <el-col :span="11"><div class="grid-content bg-purple box2">{{ this.list.content_translated}}</div></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="2"><div class="grid-content bg-purple box1">抓取人：</div></el-col>
      <el-col :span="10"><div class="grid-content bg-purple box2">{{ this.list.crawl_person}}</div></el-col>
      <el-col :span="2"><div class="grid-content bg-purple box1">抓取时间：</div></el-col>
      <el-col :span="10"><div class="grid-content bg-purple box2">{{ this.list.crawl_time}}</div></el-col>
    </el-row>
  </div>
</template>
<script>
import { sddddArticledetail} from '@/api/article'
import lineMarker from '../../components/Charts/lineMarker.vue'
export default {
  components: { lineMarker },
  name: 'Arlistdetail',
  data() {
    return {
      listLoading:true,
      list: [],
    }
  },

  created() {
    const md5 = this.$route.params && this.$route.params.md5
    console.log("md5======"+md5)
    this.getList(md5)
  },
  methods: {
    getList(md5) {
      this.listLoading = true;
      sddddArticledetail(md5).then(response =>{
          this.list=response.data;
          this.total=response.data.count;
          this.listLoading=false;
      })
    }
  }
  
}
</script>

<style scoped>
  .box1{
    text-align:center;
    line-height:40px;
    font-weight:bold;
  }
  .box2{
    line-height:40px;
    padding-left:10px;
  }
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>


