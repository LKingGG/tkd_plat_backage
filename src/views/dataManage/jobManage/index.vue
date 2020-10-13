<template>
  <div>
    <div class="mt-10 overflow">
      <el-row>
        <el-col :span="12"><el-button type="primary" icon="el-icon-plus" size="mini" class="f-l ml-10">新增</el-button></el-col>
        <el-col :span="12">
          <div>
            <el-tooltip class="item" effect="dark" content="刷新" placement="bottom">
              <el-button circle icon="el-icon-refresh" size="mini" class="f-r mr-20"></el-button>
            </el-tooltip>
          </div>
        </el-col>
      </el-row>
    </div>
    <div class="mt-10">
      <el-table
        :data="listData"
        style="width: 100%;margin-bottom: 20px;"
        row-key="id"
        border
        default-expand-all
        :tree-props="{children: 'children', hasChildren: 'hasChildren'}">
        <el-table-column
          prop="name"
          label="名称"
          width="180">
        </el-table-column>
        <el-table-column
          prop="type"
          label="类型"
          width="180">
        </el-table-column>
        <el-table-column
          prop="address"
          label="路由标识">
        </el-table-column>
        <el-table-column
          prop="address"
          label="排序">
        </el-table-column>
        <el-table-column
          prop="address"
          label="图标">
        </el-table-column>
        <el-table-column
          prop="address"
          label="菜单路径">
        </el-table-column>
        <el-table-column
          label="操作">
          <template slot-scope="scope">
            <el-button type="primary" size="mini" @click="addDialogVisible = true">新增</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!-- 添加弹窗 -->
    <el-dialog
      title="提示"
      :visible.sync="addDialogVisible"
      width="700px"
      :before-close="handleClose">
      <el-row>
        <el-col :span="12" class="mb-10">上级菜单：<el-input v-model="form.root" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12" class="mb-10">模板名称：<el-input v-model="form.name" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12" class="mb-10">模板类型：<el-input v-model="form.type" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12" class="mb-10">模板路由：<el-input v-model="form.router" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12">模板标识：<el-input v-model="form.tag" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12">模板图标：
          <el-input placeholder="点击选择图标" v-model="form.icon" class="input-with-select w-200" @click="showSelIconBtn">
            <el-button slot="append" :icon="form.icon?form.icon:'el-icon-full-screen'"></el-button>
          </el-input>
        </el-col>
      </el-row>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
    <el-dialog
      title="选择图标"
      :visible="selIconProp"
      width="700px">

    </el-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        selIconProp:false,
        listData:[{
          id: 1,
          type: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 2,
          type: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          id: 3,
          type: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄',
          children: [{
            id: 31,
            type: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄'
          }, {
            id: 32,
            type: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄'
          }]
        }, {
          id: 4,
          type: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }],
        tableData1: [{
          id: 1,
          type: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 2,
          date: '2016-05-04',
          type: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          id: 3,
          date: '2016-05-01',
          type: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄',
          hasChildren: true
        }, {
          id: 4,
          type: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }],
        addDialogVisible:false,
        addVisible:false,
        confirmLoading:false,
        form:{
          name:'',
          msg:''
        }
      };
    },
    mounted() {
      this.fetch()
    },
    methods: {
      showSelIconBtn(){
        this.selIconProp = true
      },
      handleChange(val){
        console.log(val)
      },
      addBtn(){
        this.addVisible = true
      },
      handleOk(e) {
        this.confirmLoading = true;
        setTimeout(() => {
          this.addVisible = false;
          this.confirmLoading = false;
        }, 2000);
      },
      handleCancel(e) {
        console.log('Clicked cancel button');
        this.addVisible = false;
      },
    },
  };
</script>

<style>
</style>
