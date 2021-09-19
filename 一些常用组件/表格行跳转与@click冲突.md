
### 当 `@row-click` 方法与 `@click`方法同时存在时的`冲突`问题
#### 采用 `@click.stop="() => handleMuseumInfo(row)"` 禁用父级事件的触发。

#### 具体如下:
```shell
<el-table
    :key="tableKey"
    v-loading="museumListLoading"
    :data="museumList"
    aria-setsize
    fit
    @row-click="changeExhibitionsList"
    highlight-current-row
    style="width: 100%"
  >
    <el-table-column align="center" label="名称" width="200">
      <template slot-scope="{ row }">
        <span>{{ row.name }}</span>
      </template>
    </el-table-column>

    <!-- <el-table-column width="150px" align="center" label="price">
      <template slot-scope="{ row }">
        <span>付费 {{ row.price }}</span>
      </template>
    </el-table-column> -->

    <el-table-column label="操作" align="center" width="130">
      <template slot-scope="{ row, $index }">
        <div>
          <el-button
            type="primary"
            size="mini"
            @click.stop="() => handleMuseumInfo(row)"
            >详细信息</el-button
          >
        </div>
      </template>
    </el-table-column>
  </el-table>
```

