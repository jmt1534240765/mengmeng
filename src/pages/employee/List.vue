<template>
    <div>
        <div>
            <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
            <el-button size="small" type="danger" >批量删除</el-button>
        </div>
        <el-table :data="employees">
            <el-table-column label="编号" prop='id'></el-table-column>
            <el-table-column label="姓名" prop='realname'></el-table-column>
            <el-table-column label="性别" prop='gender'></el-table-column>
            <el-table-column label="手机号" prop='telephone'></el-table-column>
            <el-table-column label="身份证号" prop='idCard'></el-table-column>
            <el-table-column label="银行卡号" prop='bankCard'></el-table-column>
            <el-table-column label="操作" >
                <template v-slot='slot'>
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler(slot.row.id)">修改</a>

                    <!-- {}显示脚本里面的数据 -->
                    <!-- {{slot.row.id}} -->
                </template>
            </el-table-column>
        </el-table>
        <!-- 分页 -->
        <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>
        <!-- 模态框 -->
        <el-dialog
            :title="录入员工信息"
            :visible.sync="visible"
            width="60%"
            >
            测试:{{form}}
<el-form label-width="80px">
  
  <el-form-item label="姓名">
    <el-input v-model="form.realname">
    </el-input>
  </el-form-item>
  <el-form-item label="性别">
    <el-radio-group v-model="form.gender">
    <el-radio label="男">男</el-radio>
    <el-radio label="女">女</el-radio>
   
  </el-radio-group>
  </el-form-item>
  <el-form-item label="手机号">
    <el-input  v-model="form.telephone">
    </el-input>
  </el-form-item>
  <el-form-item label="身份证号">
    <el-input  v-model="form.idCard">
    </el-input>
  </el-form-item>
  <el-form-item label="银行卡号">
    <el-input v-model="form.backCard">
    </el-input>
  </el-form-item>
</el-form>
            <!-- footer---按钮操作区的内容 -->
            <span slot="footer" class="dialog-footer">
                <!-- @代表的事件的绑定 ==click事件-->
                <el-button size="small" @click="submitHandler">取 消</el-button>
                <el-button  size='small' type="primary" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
        <!-- 模态框结束 -->
    </div>
</template>
<script>
import request from '@/utils/request';
import querystring from 'querystring';
export default {
   
    methods:{
         loadData(){
             //    vue实例创建完毕 
            let url="http://localhost:6677/waiter/findAll";
            request.get(url).then((response)=>{
             // 将查询结果设置到customers----数据映射, this指向外部函数的this(url)。
            this.employees=response.data;
    });
        },
         submitHandler(){
            //this.form对象-->>后台
            //调用request与后台进行交互，并且要携带参数
             let url="http://localhost:6677/employee/saveOrUpdate";
             request({
                 url,
                 method:"POST",
                 headers:{
                     "Content-Type":"application/x-www-form-urlencoded"
                 },
                 data: querystring.stringify(this.form)//转为查询字符串
             }).then((response)=>{
                 //请求结束后关闭模态框
                 this.visible=false;
                 //刷新页面
                 this.loadData();
                 //提示消息
                 this.$message({
                     type:"success",
                     message:response.message
                 })
             })
             
             
            // request.post(url,this.form)//以json形式
        },
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
             type: 'warning'
            }).then(() => {
            this.$message({
                type: 'success',
                message: '删除成功!'+id
            });
            })
        },

        toUpdateHandler(){
            this.title="修改员工信息"
            this.visible=true;
        },
        toAddHandler(){
            this.title="录入员工信息"
            this.visible=true;
        },
        closeModalHandler(){

            this.visible=false;
        }

    },
     //用于存放要向网页中显示的数据
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            // 动态模拟的数据
            employees:[],
            form:{}                 
        }
    },
 created(){
    this.loadData();
    }
}
</script>>
<style scoped>

</style>>
