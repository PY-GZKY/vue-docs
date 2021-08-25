> `Vue 图片列表预览`

## 官方文档的用法
```shell
<div class="demo-image__preview">
  <el-image 
    style="width: 100px; height: 100px"
    :src="url" 
    :preview-src-list="srcList">
  </el-image>
</div>

<script>
  export default {
    data() {
      return {
        url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
        srcList: [
          'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
          'https://fuss10.elemecdn.com/1/8e/aeffeb4de74e2fde4bd74fc7b4486jpeg.jpeg'
        ]
      }
    }
  }
</script>
```


## `ElImageViewer` 组件使用方法

```shell
<el-image-viewer
  v-if="showViewer"
  :initial-index="1"
  :on-close="onClose"
  :on-switch="onSwitch"
  :url-list="urlList"
/>

// script
import ElImageViewer from 'element-ui/packages/image/src/image-viewer'
export default {
  components: {
    ElImageViewer
  },
  data() {
    return {
      // 是否显示
      showViewer: false,
      urlList: [
        'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
        'https://fuss10.elemecdn.com/1/8e/aeffeb4de74e2fde4bd74fc7b4486jpeg.jpeg'
      ]
    }
  },
  methods: {
    // 关闭图片预览
    onClose() {
      this.showViewer = false
    },
    // 切换图片 index为图片下标值
    onSwitch(index) {
      console.log(index)
    }
  }
}
```

- `initialIndex`	默认显示的第一张预览图（urlList的下标值）
- `urlList`	预览图的地址列表
- `zIndex`	设置图片预览的 z-index	Number
- `onClose`	关闭图片预览时的回调函数	Function	
- `onSwitch`	切换上一张下一张图片时的回调函数


```shell
<template>
  <div class="app-container">
    <div class="filter-container">
      <Comments></Comments>
      <el-input
        v-model="listQuery.city"
        placeholder="输入城市"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="handleFilter"
      />

      <el-input
        v-model="listQuery.content"
        placeholder="评论内容"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="handleFilter"
      />

      <el-input
        v-model="listQuery.scenic"
        placeholder="景点名称"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="handleFilter"
      />

      <el-button
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-search"
        @click="handleFilter"
      >
        搜索
      </el-button>

      <el-button
        v-waves
        :loading="downloadLoading"
        class="filter-item"
        type="primary"
        icon="el-icon-download"
        @click="handleDownload"
      >
        导出
      </el-button>
      <el-button
        v-waves
        :loading="downloadLoading"
        class="filter-item"
        type="success"
        icon="el-icon-plus"
        @click="handleCreate"
      >
        新增评论
      </el-button>

      <el-button
        type="danger"
        class="filter-item"
        icon="el-icon-delete"
        :disabled="multiple"
        @click="handleManyDelete"
        >批量操作
      </el-button>

      <el-checkbox
        class="filter-item"
        style="margin-left: 15px"
        @change="handleFilterContent30()"
      >
        只看字数大于30的评论内容
      </el-checkbox>
    </div>

    <el-table
      :key="tableKey"
      v-loading="listLoading"
      :data="list"
      border
      fit
      highlight-current-row
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55" align="center" />

      <el-table-column label="城市" prop="id" align="center" width="110">
        <template slot-scope="{ row }">
          <span>{{ row.city }}</span>
        </template>
      </el-table-column>

      <el-table-column label="景点名称" width="110px" align="center">
        <template slot-scope="{ row }">
          <span>{{ row.scenic }}</span>
        </template>
      </el-table-column>

      <el-table-column width="120px" align="center" label="用户昵称">
        <template slot-scope="{ row }">
          <span>{{ row.username }}</span>
        </template>
      </el-table-column>

      <el-table-column
        show-overflow-tooltip
        width="600px"
        align="center"
        label="评论内容"
      >
        <template slot-scope="{ row }">
          <span>{{ row.content }}</span>
        </template>
      </el-table-column>

      <el-table-column label="发布时间" width="200px" align="center">
        <template slot-scope="{ row }">
          <span>{{ row.pub_time }}</span>
        </template>
      </el-table-column>

      <el-table-column label="更新时间" width="200px" align="center">
        <template slot-scope="{ row }">
          <span>{{ row.update_time }}</span>
        </template>
      </el-table-column>

      <el-table-column label="数据来源" width="200px" align="center">
        <template slot-scope="{ row }">
          <span>{{ row.data_sources }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Actions" align="center" width="320">
        <template slot-scope="{ row, $index }">
          <el-button
            v-if="row.new_img_list.length != 0"
            size="mini"
            type="success"
            @click="handleImgList(row)"
          >
            查看图集
          </el-button>
          <!-- <el-button v-else size="mini" type="success">
            占位
          </el-button> -->

          <el-button type="primary" size="mini" @click="handleUpdate(row)">
            编辑
          </el-button>

          <el-button
            v-if="row.status != 'deleted'"
            size="mini"
            type="danger"
            @click="handleDelete(row, $index)"
          >
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <pagination
      v-show="total > 0"
      :total="total"
      :page.sync="listQuery.page"
      :limit.sync="listQuery.limit"
      @pagination="getList"
    />

    <el-image-viewer
      v-if="showViewer"
      :initial-index="1"
      :on-close="onClose"
      :on-switch="onSwitch"
      :url-list="nowRow.new_img_list"
    />

    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form
        ref="dataForm"
        :rules="rules"
        :model="temp"
        label-position="left"
        label-width="85px"
        style="width: 600px; margin-left: 25px"
      >
        <el-form-item label="城市" prop="city">
          <el-input v-model="temp.city" />
        </el-form-item>
        <el-form-item label="景点名称" prop="scenic">
          <el-input v-model="temp.scenic" />
        </el-form-item>
        <el-form-item label="用户昵称" prop="username">
          <el-input v-model="temp.username" />
        </el-form-item>

        <el-form-item label="评论内容" prop="content">
          <el-input
            v-model="temp.content"
            :autosize="{ minRows: 4, maxRows: 8 }"
            type="textarea"
            placeholder="评论内容"
          />
        </el-form-item>

        <el-form-item label="发布时间" prop="pub_time">
          <el-date-picker
            v-model="temp.pub_time"
            type="datetime"
            placeholder="选择时间"
            value-format="yyyy-MM-dd HH:mm:ss"
          />
        </el-form-item>

        <!-- <el-form-item label="Status">
          <el-select
            v-model="temp.status"
            class="filter-item"
            placeholder="Please select"
          >
            <el-option
              v-for="item in statusOptions"
              :key="item"
              :label="item"
              :value="item"
            />
          </el-select>
        </el-form-item> -->
        <el-form-item label="数据来源" prop="data_sources">
          <el-input v-model="temp.data_sources" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false"> 取消 </el-button>
        <el-button
          type="primary"
          @click="dialogStatus === 'create' ? createData() : updateData()"
        >
          确认
        </el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import {
  get_mafengwo_list,
  create_mafengwo_comment,
  update_mafengwo_comment,
  delete_mafengwo_comment,
} from "@/api/comment";
import waves from "@/directive/waves"; // waves directive
import Pagination from "@/components/Pagination"; // secondary package based on el-pagination
import Comments from "./head";
// import ElImageViewer from "element-ui/packages/image/src/image-viewer";

export default {
  name: "",
  components: { Pagination },
  directives: { waves },
  data() {
    return {
      tableKey: 0,
      list: null,
      total: 0,
      listLoading: true,
      // 选中数组
      ids: [],
      // 非单个禁用
      single: true,
      // 非多个禁用
      multiple: true,
      textMap: {
        update: "Edit",
        create: "Create",
      },
      listQuery: {
        page: 1,
        limit: 15,
        city: "",
        content: "",
        scenic: "",
        isContent30: false,
      },
      temp: {
   
      },
      nowRow: "",
      first_img: "",
      currentAudioName: "",
      cityOptions: ["纽约", "巴黎", "北京"],
      showAudio: false,
      dialogFormVisible: false,
      dialogStatus: "",
      showViewer: false,
      rules: {
        city: [
          { required: true, message: "city is required", trigger: "change" },
        ],
        scenic: [
          { required: true, message: "scenic is required", trigger: "change" },
        ],
        username: [
          {
            required: true,
            message: "username is required",
            trigger: "change",
          },
        ],
        content: [
          { required: true, message: "content is required", trigger: "change" },
        ],
        pub_time: [
          {
            required: true,
          },
        ],
        data_sources: [
          {
            required: true,
            message: "data_sources is required",
            trigger: "change",
          },
        ],
      },
      downloadLoading: false,
    };
  },
  components: {
    Comments,
    // ElImageViewer,
  },
  created() {
    this.getList();
  },
  methods: {
    // 关闭图片预览
    onClose() {
      this.showViewer = false;
    },
    // 切换图片 index为图片下标值
    onSwitch(index) {
      // console.log(index)
    },

    // 多选框选中数据
    handleSelectionChange(selection) {
      this.ids = selection.map((item) => item.id);
      this.single = selection.length !== 1;
      this.multiple = !selection.length;
    },

    // this.listQuery.isContent30 = !this.listQuery.isContent30;
    getList() {
      this.listLoading = true;
      console.log("listQuery: ", this.listQuery);
      get_mafengwo_list(this.listQuery).then((response) => {
        console.log("response: ", response);
        this.list = response.data.items;
        this.total = response.data.total;
        // Just to simulate the time of the request
        setTimeout(() => {
          this.listLoading = false;
        }, 1.5 * 1000);
      });
    },

    // 我真是太聪明了
    handleFilterContent30() {
      this.listQuery.isContent30 = !this.listQuery.isContent30;
      this.getList();
    },

    handleFilter() {
      this.getList();
    },
    // 图集
    handleImgList(row) {
      this.showViewer = true;
      this.nowRow = row;
    },

    resetTemp() {
      this.temp = {
        city: "北京",
        scenic: "天安门广场",
        username: "鬼子口音",
        content: "这是一段内容",
        comment_img: [],
        data_sources: "马蜂窝评论",
        new_img_list: [],
        pub_time: this.dayjs().format("YYYY-MM-DD HH:mm:ss"),
        update_time: this.dayjs().format("YYYY-MM-DD HH:mm:ss"),
        user_home: "",
      };
    },
    handleCreate() {
      this.resetTemp();
      this.dialogStatus = "create";
      this.dialogFormVisible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].clearValidate();
      });
    },
    createData() {
      this.$refs["dataForm"].validate((valid) => {
        if (valid) {
          console.log("this.temp:", this.temp);
          // this.temp.id = parseInt(Math.random() * 100) + 1024; // mock a id
          // this.temp.author = "vue-element-admin";
          create_mafengwo_comment(this.temp).then(() => {
            this.dialogFormVisible = false;
            this.$notify({
              title: "Success",
              message: "创建成功",
              type: "success",
              duration: 2000,
            });
          });
        }
      });
    },
    handleUpdate(row) {
      this.temp = Object.assign({}, row); // copy obj
      // console.log(new Date(1280977330000));
      // this.temp.timestamp = new Date(this.temp.timestamp);
      this.dialogStatus = "update";
      this.dialogFormVisible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].clearValidate();
      });
    },
    updateData() {
      this.$refs["dataForm"].validate((valid) => {
        if (valid) {
          const tempData = Object.assign({}, this.temp);
          // tempData.timestamp = +new Date(tempData.timestamp); // change Thu Nov 30 2017 16:41:05 GMT+0800 (CST) to 1512031311464
          update_mafengwo_comment(tempData).then(() => {
            // const index = this.list.findIndex((v) => v.id === this.temp.id);
            // this.list.splice(index, 1, this.temp);
            this.dialogFormVisible = false;
            this.$notify({
              title: "Success",
              message: "更新成功",
              type: "success",
              duration: 2000,
            });
            this.getList();
          });
        }
      });
    },

    handleDelete(row, index) {
      this.$notify({
        title: "error",
        message: "不支持删除操作",
        type: "error",
        duration: 2000,
      });
      this.getList();
    },

    /** 批量删除 */
    handleManyDelete() {
      const ids = this.ids;
      this.$confirm("是否确认批量删除数据项?", "警告", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(function () {
          // 批量删除操作
          return 1;
        })
        .then(() => {
          this.getList();
          this.msgSuccess("删除成功");
        })
        .catch(function () {});
    },

    handleDownload() {
      this.downloadLoading = true;
      import("@/vendor/Export2Excel").then((excel) => {
        const tHeader = [
          "城市",
          "景点名称",
          "用户昵称",
          "评论内容",
          // "图片集合",
          "发布时间",
          "更新时间",
          "数据来源",
        ];
        const filterVal = [
          "city",
          "scenic",
          "username",
          "content",
          // "comment_img",
          "pub_time",
          "update_time",
          "data_sources",
        ];
        const data = this.formatJson(filterVal);
        var date = new Date();
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename:
            "马蜂窝" + date.toLocaleString("chinese", { hour12: false }),
        });
        this.downloadLoading = false;
      });
    },
    formatJson(filterVal) {
      return this.list.map((v) =>
        filterVal.map((j) => {
          if (j === "timestamp") {
            return parseTime(v[j]);
          } else {
            return v[j];
          }
        })
      );
    },

    // 音频播放前将名称放出来
    handleBeforePlay(row) {
      // 这里可以做一些事情...
      this.currentAudioName = row.讲解点名称_中文;
    },
  },
};
</script>
```

