<template>
  <el-container>
    <el-header>
      <h1>监控回放系统</h1>
    </el-header>
    <el-main>
      <!-- 卡片标签页 -->
      <el-tabs type="border-card">
        <!-- 日志展示区域 -->
        <el-tab-pane label="日志展示">
          <!-- 包展示表格信息 -->
          <el-table :data="packageData" border stripe>
            <el-table-column type="index"></el-table-column>
            <el-table-column
              label="商品名称"
              prop="goods_name"
            ></el-table-column>
            <el-table-column
              label="商品价格(元)"
              prop="goods_price"
            ></el-table-column>
            <el-table-column
              label="商品重量"
              prop="goods_weight"
            ></el-table-column>
            <el-table-column label="创建时间" prop="add_time">
              <template slot-scope="scope">
                {{ scope.row.add_time }}
              </template>
            </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button
                  type="primary"
                  icon="el-icon-edit"
                  size="mini"
                ></el-button>
                <el-button
                  type="danger"
                  icon="el-icon-delete"
                  size="mini"
                  @click="removeById(scope.row.goods_id)"
                ></el-button>
              </template>
            </el-table-column>
          </el-table>
          <!-- 包表格分页信息 -->
          <el-pagination
            @size-change="packageHandleSizeChange"
            @current-change="packageHandleSCurrentChange"
            :current-page="packageQueryInfo.pagenum"
            :page-sizes="[5, 10, 15, 20]"
            :page-size="packageQueryInfo.pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="packageTotal"
            background
          >
          </el-pagination>
        </el-tab-pane>
        <!-- 回放展示区域 -->
        <el-tab-pane label="回放展示">
          <el-steps :active="stepIndex" align-center process-status="wait">
            <el-step
              title="步骤 1"
              icon="el-icon-monitor"
              description="balabala"
            ></el-step>
            <el-step
              title="步骤 2"
              icon="el-icon-s-platform"
              description="balabala"
            ></el-step>
            <el-step
              title="步骤 3"
              icon="el-icon-picture"
              description="balabala"
            ></el-step>
          </el-steps>
          <el-card>
            <!-- 步骤展示表格信息 -->
            <el-table :data="stepData" border stripe>
              <el-table-column type="index"></el-table-column>
              <el-table-column
                label="商品名称"
                prop="goods_name"
              ></el-table-column>
              <el-table-column
                label="商品价格(元)"
                prop="goods_price"
              ></el-table-column>
              <el-table-column
                label="商品重量"
                prop="goods_weight"
              ></el-table-column>
              <el-table-column label="创建时间" prop="add_time">
                <template slot-scope="scope">
                  {{ scope.row.add_time }}
                </template>
              </el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button
                    type="primary"
                    icon="el-icon-edit"
                    size="mini"
                  ></el-button>
                  <el-button
                    type="danger"
                    icon="el-icon-delete"
                    size="mini"
                    @click="removeById(scope.row.goods_id)"
                  ></el-button>
                </template>
              </el-table-column>
            </el-table>
            <!-- 步骤表格分页信息 -->
            <el-pagination
              @size-change="packageHandleSizeChange"
              @current-change="packageHandleSCurrentChange"
              :current-page="packageQueryInfo.pagenum"
              :page-sizes="[5, 10, 15, 20]"
              :page-size="packageQueryInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="packageTotal"
              background
            >
            </el-pagination>
          </el-card>
        </el-tab-pane>
      </el-tabs>
    </el-main>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      // 日志展示区表格数据查询参数对象信息
      packageQueryInfo: {
        query: '',
        pagenum: 1,
        pagesize: 2
      },
      // 日志展示区表格数据
      packageData: [],
      // 日志展示区表格总条目
      packageTotal: 0,
      // 分页区展示页码数量最大值
      pageCount: 7,
      // 回放展示区step当前激活index
      stepIndex: 1,
      // 回放展示区表格数据
      stepData: [],
      // 回放展示区表格数据查询参数对象信息
      stepQueryInfo: {
        query: '',
        pagenum: 1,
        pagesize: 2
      },
      // 日志展示区表格总条目
      stepTotal: 0,
      // 分页区展示页码数量最大值
      stepCount: 7
    }
  },
  created() {
    this.packageTotal = 10
    this.getPackageData()
    this.getStepData()
  },
  methods: {
    async getPackageData() {
      const { data: res } = await this.$http.get('goods', {
        params: this.packageQueryInfo
      })

      if (res.meta.status !== 200) {
        return this.$message.error('获取商品列表失败！')
      }

      this.$message.success('获取商品列表成功！')
      console.log(res.data)
      this.packageData = res.data.goods
      this.packageTotal = res.data.packageTotal
    },
    packageHandleSizeChange(newSize) {
      this.packageQueryInfo.pagesize = newSize
      this.getPackageData()
    },
    packageHandleSCurrentChange(newPage) {
      this.packageQueryInfo.pagenum = newPage
      this.getPackageData()
    },
    async getStepData() {
      const { data: res } = await this.$http.get('goods', {
        params: this.stepQueryInfo
      })

      if (res.meta.status !== 200) {
        return this.$message.error('获取商品列表失败！')
      }

      this.$message.success('获取商品列表成功！')
      console.log(res.data)
      this.stepData = res.data.goods
      this.stepTotal = res.data.packageTotal
    },
    stepHandleSizeChange(newSize) {
      this.stepQueryInfo.pagesize = newSize
      this.getStepData()
    },
    stepHandleSCurrentChange(newPage) {
      this.stepQueryInfo.pagenum = newPage
      this.getStepData()
    }
  }
}
</script>

<style scoped>
.el-header {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  height: 20% !important;
  display: flex;
  align-items: center;
  justify-content: center;
}

.el-main {
  padding: 0 !important;
  background-color: #e9eef3;
  color: #333;
  text-align: center;
}

.el-container {
  height: 100%;
  margin-bottom: 40px;
}
.el-tabs {
  height: 100%;
  font-size: 16px !important;
}
.el-steps {
  margin-top: 20px !important;
  margin-bottom: 20px !important;
}
</style>