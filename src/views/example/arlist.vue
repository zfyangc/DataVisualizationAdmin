<template>
  <div class="app-container">
    <div class="filter-container">
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item label="时间：">
          <el-date-picker
            v-model="pldate"
            type="date"
            placeholder="选择日期"
            class="pldatedata"
          >
          </el-date-picker>
        </el-form-item>
        <el-form-item label="网站：">
          <el-select v-model="country" placeholder="请选择" class="sleop">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            >
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" icon="el-icon-search" @click="onSubmit"
            >查询</el-button
          >
        </el-form-item>
        <el-form-item>
          <el-button
            :loading="downloadLoading"
            class="filter-item"
            type="primary"
            icon="el-icon-download"
            @click="handleDownload"
            >导出</el-button
          >
          <!-- <el-button
            v-waves
            :loading="downloadLoading"
            class="filter-item"
            type="primary"
            icon="el-icon-star-on"
            @click="handleDownload"
            >标记</el-button
          > -->
        </el-form-item>
      </el-form>
    </div>
    <el-table
      v-loading="listLoading"
      :data="list"
      border
      fit
      highlight-current-row
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="40"> </el-table-column>
      <el-table-column align="center" label="标题" min-width="150">
        <template slot-scope="scope" style="width: 10%; background: #00ff00">
          <router-link
            :to="'/example/arlistdetail/' + scope.row.md5"
            class="link-type"
          >
            <span>{{ scope.row.title }}</span>
          </router-link>
        </template>
      </el-table-column>
      <el-table-column
        min-width="250"
        align="center"
        :show-overflow-tooltip="true"
        label="摘要"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.abstract }}</span>
        </template>
      </el-table-column>
      <el-table-column min-width="60" align="center" label="来源">
        <template slot-scope="scope">
          <span>{{ scope.row.website_alias }}</span>
        </template>
      </el-table-column>
      <el-table-column
        min-width="150"
        align="center"
        :show-overflow-tooltip="true"
        label="URL"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.url }}</span>
        </template>
      </el-table-column>
      <el-table-column min-width="70" align="center" label="时间">
        <template slot-scope="scope">
          <span>{{ scope.row.time }}</span>
        </template>
      </el-table-column>
      <!-- <el-table-column min-width="60" align="center" label="是否下载">
        <template slot-scope="scope">
          <el-switch
            style="display: block"
            v-model="value2"
            active-color="#ff4949"
            inactive-color="#13ce66"
            active-text="否"
            inactive-text="是">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column min-width="60" align="center" label="是否重要">
        <template slot-scope="scope">
          <el-switch
            style="display: block"
            v-model="value2"
            active-color="#ff4949"
            inactive-color="#13ce66"
            active-text="否"
            inactive-text="是">
          </el-switch>
        </template>
      </el-table-column> -->
      <el-table-column
        label="操作"
        align="center"
        min-width="50"
        class-name="small-padding fixed-width"
      >
        <template slot-scope="scope">
          <el-button
            type="primary"
            size="mini"
            @click="handleardetail(scope.row)"
            >详情</el-button
          >
          <!-- <el-button
            v-if="scope.row.status != 'published'"
            size="mini"
            type="success"
            @click="handleModifyStatus(scope.row, 'published')"
            >{{ $t("table.publish") }}
          </el-button> -->
          <!-- <el-button
            v-if="scope.row.status != 'draft'"
            size="mini"
            @click="handleModifyStatus(scope.row, 'draft')"
            >{{ $t("table.draft") }}
          </el-button> -->
          <!-- <el-button
            v-if="scope.row.status != 'deleted'"
            size="mini"
            type="danger"
            @click="handleModifyStatus(scope.row, 'deleted')"
            >{{ $t("table.delete") }}
          </el-button> -->
        </template>
      </el-table-column>
    </el-table>
    <pagination
      v-show="total > 10"
      :total="total"
      :page.sync="listQuery.page"
      :limit.sync="listQuery.limit"
      @pagination="getList"
      class="pageclass"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      layout="total, sizes, prev, pager, next, jumper"
    />
    <el-dialog :title="ardetaildia" :visible.sync="dialogPvVisible" v-if='dialogPvVisible' width='70%'>
      <el-row :gutter="20">
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">md5：</div></el-col
        >
        <el-col :span="10"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.md5 }}
          </div></el-col
        >
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">新闻时间：</div></el-col
        >
        <el-col :span="10"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.time }}
          </div></el-col
        >
      </el-row>
      <el-row :gutter="20">
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">URL：</div></el-col
        >
        <el-col :span="10"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.url }}
          </div></el-col
        >
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">网站名称：</div></el-col
        >
        <el-col :span="10"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.website }}
          </div></el-col
        >
      </el-row>
      <el-row :gutter="20">
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">标题：</div></el-col
        >
        <el-col :span="11"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.title }}
          </div></el-col
        >
        <el-col :span="11"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.title_translated }}
          </div></el-col
        >
      </el-row>
      <el-row :gutter="20">
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">摘要：</div></el-col
        >
        <el-col :span="11"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.abstract }}
          </div></el-col
        >
        <el-col :span="11"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.abstract_translated }}
          </div></el-col
        >
      </el-row>
      <el-row :gutter="20">
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">内容：</div></el-col
        >
        <el-col :span="11"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.content }}
          </div></el-col
        >
        <el-col :span="11"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.content_translated }}
          </div></el-col
        >
      </el-row>
      <el-row :gutter="20">
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">抓取人：</div></el-col
        >
        <el-col :span="10"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.crawl_person }}
          </div></el-col
        >
        <el-col :span="2"
          ><div class="grid-content bg-purple box1">抓取时间：</div></el-col
        >
        <el-col :span="10"
          ><div class="grid-content bg-purple box2">
            {{ this.listdetail.crawl_time }}
          </div></el-col
        >
      </el-row>
    </el-dialog>
  </div>
</template>

<script>
import { sddddArticle, sddddArticledetail } from "@/api/article";
import Pagination from "@/components/Pagination"; // Secondary package based on el-pagination
import waves from "@/directive/waves";

export default {
  name: "ArList",
  components: { Pagination },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: "success",
        draft: "info",
        deleted: "danger",
      };
      return statusMap[status];
    },
  },
  data() {
    return {
      showing: true,
      hiddening: false,
      list: null,
      total: 0,
      listLoading: true,
      currentPage4: 1,
      listQuery: {
        page: 1,
        limit: 20,
      },
      pldate: "",
      options: [
        {
          value: "yd",
          label: "印度",
        },
        {
          value: "yn",
          label: "越南",
        },
        {
          value: "oz",
          label: "澳洲",
        },
        {
          value: "mg",
          label: "美国",
        },
      ],
      country: "",
      value1: true,
      value2: true,
      listdetail: null,
      dialogPvVisible: false,
      ardetaildia: "",
    };
  },
  created() {
    this.getList();
  },
  methods: {
    handleSizeChange(val) {
      this.listQuery.limit = val;
      console.log("111111");
      this.getList();
    },
    handleCurrentChange(val) {
      console.log("22222");
      this.listQuery.page = val;
      this.getList();
    },
    getList() {
      // this.list = null;
      this.listLoading = true;
      console.log("this.listQuery.page====" + this.listQuery.page);
      console.log("this.listQuery.limit=----" + this.listQuery.limit);
      sddddArticle({
        page: this.listQuery.page,
        page_size: this.listQuery.limit,
      }).then((response) => {
        this.list = response.data.results;
        this.total = response.data.count;
        this.listLoading = false;
        console.log(response);
      });
    },
    handleardetail(row) {
      console.log(row);
      this.dialogPvVisible = true;
      sddddArticledetail(row.md5).then((response) => {
        this.ardetaildia = response.data.title;
        this.listdetail = response.data;
        console.log(this.listdetail);
      });
      
    },
  },
};
</script>

<style scoped>
.aaa {
  background-color: aqua;
}
.box1 {
  text-align: center;
  line-height: 40px;
  font-weight: bold;
}
.edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;
}
.el-header {
  /* background-color: #E9EEF3; */
  color: #333;
  line-height: 40px;
}

.el-aside {
  color: #333;
}
.pageclass {
  padding: 6px 16px;
  margin-top: 8px;
}
.el-aside-select {
  margin-top: 30px;
}
.pldatedata {
  width: 100%;
}
.sleop {
  width: 100%;
}
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
