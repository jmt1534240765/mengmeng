<template>
    <div>
        <!-- 按钮 -->
       <el-button  type="success" size="small" @click="toAddHandler">添加</el-button>
       <el-button type="danger" size="small" >批量删除</el-button>
       <!-- /按钮 -->
         
        <!-- 表格 -->
       <!-- :data 动态绑定值 -->
       <el-table :data="customers">
           
           <el-table-column prop="id" label="编号"></el-table-column>
           <el-table-column prop="realname" label="姓名"></el-table-column>
           <el-table-column prop="telephone" label="联系方式"></el-table-column>
           <el-table-column label="操作">
               <template v-slot="slot">
                   <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                   <!-- @click.prevent阻止默认行为 ----一般a标签用prevent-->
                   <a href="" @click.prevent="toUpdateHandler">修改</a>
               </template>
           </el-table-column>
           
       </el-table>
       <!-- /表格 -->
       <!-- 分页 -->
       <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>
        <!-- 分页结束 -->
        <!-- 模态框 -->
        <el-dialog
            title="录入顾客信息"
            :visible.sync="visible"
            width="30%"
            >
            <el-form :model="form" label-width="80px" :label-position="right" size="mini">
                <el-form-item label="用户名">
                    <el-input v-model="form.username" ></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input type="password" v-model="form.password"></el-input>
                </el-form-item>
                <el-form-item label="真实姓名">
                    <el-input  v-model="form.realname"></el-input>
                </el-form-item>
                <el-form-item label="联系方式">
                    <el-input  v-model="form.telephone"></el-input>
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
    // methods用于存放网页中需要调用的方法

    methods:{
        
        loadData(){
             //    vue实例创建完毕 
            let url="http://localhost:6677/customer/findAll";
            request.get(url).then((response)=>{
             // 将查询结果设置到customers----数据映射, this指向外部函数的this(url)。
            this.customers=response.data;
    });
        },
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
             type: 'warning'
            }).then(() => {
                let url ="http://localhost:6677/custom/deleteById"+id;
                request.get()
            this.$message({
                
                type: 'success',
                message: response.message
            });
            })
        },

        toUpdateHandler(row){
            this.visible=true;
            //模态框显示当前的信息
            this.form = row;
        },
        toAddHandler(){
           
            this.visible=true;
        },
        submitHandler(){
            //this.form对象-->>后台
            //调用request与后台进行交互，并且要携带参数
             let url="http://localhost:6677/customer/saveOrUpdate";
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
        closeModalHandler(){

            this.visible=false;
        }

    },
    //用于存放要向网页中显示的数据
    data(){
        return{
            visible:false,
            // 动态模拟的数据
            customers:[],
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