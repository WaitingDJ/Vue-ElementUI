<template>
    <div>
        <el-card class="box-card">
            <div slot="header" class="clearfix">
                <span style="float:left;">卡片名称</span>
            </div>
            <el-row type="flex" align="middle" :gutter="20">
                <el-col :span="5">已选择5个活动</el-col>
                <el-col :span="5">
                    <el-select v-model="currentType" placeholder="请选择">
                        <el-option v-for="type in types" :value="type" :key="type"></el-option>
                    </el-select>
                </el-col>
                <el-col :span="13">
                    <el-button type="danger" :plain="true" @click="dialogVisible = !dialogVisible">删除</el-button>
                    <el-button type="info" :plain="true">置顶</el-button>
                    <el-button type="info" :plain="true">复制</el-button>
                </el-col>
            </el-row>
            <el-table :data="tableData" stripe style="width: 100%" @selection-change="selectionChange">
                <el-table-column type="selection" width="50"> </el-table-column>
                <el-table-column prop="title" label="活动名称"> </el-table-column>
                <el-table-column prop="type" label="活动类型"> </el-table-column>
                <el-table-column prop="status"  label="活动状态"> </el-table-column>
                <el-table-column prop="readNum" label="浏览数"> </el-table-column>
                <el-table-column prop="signUpNum" label="报名数"> </el-table-column>
                <el-table-column prop="auditNum" label="待审核"> </el-table-column>
                <el-table-column prop="id" label="操作">
                    <el-button>默认按钮</el-button>
                </el-table-column>
            </el-table>
        </el-card>
        <el-dialog title="提示" :visible.sync="dialogVisible" width="30%">
            <span>这是一段信息</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="removeHandle">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "HelloWorld",
  data() {
    return {
      currentType: "免费",
      types: ['免费','不免费','想免费','想的美'],
      tableData: [],
      dialogVisible: false,
      selectionItems: []
    };
  },
  created () {
      axios.get("/static/json/test.json").then( (res) => {
          console.log(res);
          this.tableData = res.data.list;
      }).catch( (err) => {
          console.log(err);
      })
  },
  methods:{
      selectionChange: function(val){
          var arr = [];
          val.forEach(item => {
              arr.push(item);
          });
          this.selectionItems = arr;
      },
      removeHandle: function(){
          var tableData = this.tableData;
          this.selectionItems.forEach(function(id){
              tableData.forEach(function(data){
                  if(id.id == data.id){
                      tableData.splice(tableData.indexOf(data),1)
                  }
              })
          })
          this.selectionItems = [];
          this.dialogVisible = false;
      }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}
</style>
