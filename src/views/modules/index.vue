<template>
  <div>
    <div class="mt-10 overflow">
      <el-row>
        <el-col :span="12"><el-button type="primary" icon="el-icon-plus" size="mini" class="f-l ml-10" @click="showAddDialog">新增</el-button></el-col>
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
          align="center"
          width="180">
        </el-table-column>
        <el-table-column
          prop="type"
          label="类型"
          align="center"
          width="180">
        </el-table-column>
        <el-table-column
          prop="address"
          align="center"
          label="路由标识">
        </el-table-column>
        <el-table-column
          prop="address"
          align="center"
          label="排序">
        </el-table-column>
        <el-table-column
          prop="address"
          align="center"
          label="图标">
        </el-table-column>
        <el-table-column
          prop="address"
          align="center"
          label="菜单路径">
        </el-table-column>
        <el-table-column
          label="操作"
          align="center">
          <template slot-scope="scope">
            <el-button type="text" icon="el-icon-edit" class="mr-10">编辑</el-button>
            <el-popconfirm
              title="确定要删除吗？"
            >
              <el-button slot="reference" type="text" icon="el-icon-delete">删除</el-button>
            </el-popconfirm>
            <br/>
            <el-button type="text" icon="el-icon-plus" size="mini" @click="addChild(scope.row)">添加下级菜单</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!-- 添加弹窗 -->
    <el-dialog
      title="添加"
      :visible.sync="addDialogVisible"
      width="700px"
      :before-close="handleClose">
      <el-row>
        <el-col :span="12" class="mb-10">上级菜单：<el-input v-model="form.father" size="size" class="w-200" readonly></el-input></el-col>
        <el-col :span="12" class="mb-10">模板名称：<el-input v-model="form.name" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12" class="mb-10" style="line-height: 40px">模板类型：<el-radio v-if="form.isRoot" v-model="form.type" label="0">文件夹</el-radio>
          <el-radio v-else v-model="form.type" label="1">按钮</el-radio>
          <el-radio v-model="form.type" label="2">页面</el-radio></el-col>
        <el-col :span="12" class="mb-10" v-if="form.type != 0">模板路由：<el-input v-model="form.router" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12">模板标识：<el-input v-model="form.tag" size="size" class="w-200"></el-input></el-col>
        <el-col :span="12">模板图标：
          <el-input placeholder="点击选择图标" v-model="form.icon" class="input-with-select w-200" @onblur="showSelIconBtn">
            <el-button slot="append" :icon="form.icon?form.icon:'el-icon-full-screen'" @click="showSelIconBtn"></el-button>
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
      <div class="iconListDiv">
        <div v-for="(item,index) in iconList" class="iconList" @click="selIcon(item)">
          <i :class="item"></i>
          <p>{{item}}</p>
        </div>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  import iconList from '@/views/modules/iconList.json'
  export default {
    data() {
      return {
        selIconProp:false,
        iconList:iconList,
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
          tag:'',
          type:'2',
          father:'',
          icon:'',
          isRoot:true
        }
      };
    },
    mounted() {
      this.fetch()
    },
    methods: {
      delBtn(row){
        console.log(row)
      },
      addChild(row){
        this.form.father = row.name
        this.form.isRoot = false
        this.addDialogVisible = true
      },
      selIcon(icon){
        this.form.icon = icon
        this.selIconProp = false
      },
      showAddDialog(){
        this.form.father = "顶级"
        this.form.isRoot = true
        this.addDialogVisible = true
      },
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

<style scoped>
  .iconListDiv{
    overflow: hidden;
  }
  .iconList{
    float: left;
    width: 112px;
    height: 90px;
    box-sizing: border-box;
    padding: 10px;
    text-align: center;
    border: 1px #999 dashed;
    border-radius: 8px;
    margin: 4px 0;
  }
  .iconList:hover{
    cursor: pointer;
    border-color: dodgerblue;
    color: dodgerblue;
  }
  .iconList:hover i{
    font-size: 34px;
  }
  .iconList i{
    font-size: 30px;
  }
  .iconList p{
    margin-top: 8px;
  }
  .iconListDiv .iconList:not(:nth-child(5n)){
    margin: 4px 20px 4px 0;
  }
</style>
