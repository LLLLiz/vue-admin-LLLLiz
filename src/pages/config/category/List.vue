<template>
    <div>
        <!-- 按钮 -->
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>
        
        <!-- /按钮 -->
        <!-- 表格 -->
        <el-table :data="categorys">
            
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="产品名称"></el-table-column>
            <el-table-column prop="num" label="数目"></el-table-column>
            <el-table-column prop="parentId" label="上层id"></el-table-column>
            
            <el-table-column label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler">修改</a>
                </template></el-table-column>
            
        </el-table>
        <!-- /表格 -->
        <!-- 分页 -->
        <!-- <el-pagination layout="prev, pager, next" :total="50"></el-pagination> -->
        <!-- /分页 -->
        <!-- 模态框 -->
        <el-dialog
        title="修改栏目信息"
        :visible.sync="visible"
        width="60%">
        <el-form :model="form" label-width="80px">
            <el-form-item label="编号">
                <el-input v-model="form.id"/>
            </el-form-item>
            <el-form-item label="产品名称">
                <el-input v-model="form.name"/>
            </el-form-item>
            <el-form-item label="数目">
                <el-input v-model="form.num"/>
            </el-form-item>
            <el-form-item label="上层id">
              <el-input v-model="form.parentId"/>
            </el-form-item>

            

        </el-form>
      
      <span slot="footer" class="dialog-footer">
        <el-button size="small" @click="closeModalHandler">取 消</el-button>
        <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
      </span>
    </el-dialog>
        <!-- /模态框 -->
    </div>
</template>
<script>
import request from'@/utils/request'
import querystring from 'querystring'
export default {//暴露接口
    data(){
        return{
            checked:true,
            title:"录入栏位信息",
            visible:false,
            categorys:[],
            form:{
                type:"category"
            }
        }
    },
    created(){
        //在页面加载出来时加载数据
        this.loadData();
    },
    methods:{
        submitHandler(){
        let url = "http://localhost:6677/category/saveOrUpdate"
        request({
        url,
        method:"post",
        headers:{
            "Content-Type":"application/x-www-form-urlencoded"
            },
            data:querystring.stringify(this.form)
        }).then((response)=>{
                //模态框关闭
                this.closeModalHandler();
                //刷新
                this.loadData();
                //关闭模态框
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
        },
        loadData(){
            //this ->vue实例，通过vue实例访问该实例中数据
            //this.title/this.toAddhandler
            let url = "http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
                this.categorys = response.data; 
            }) 
        },
        
        toAddHandler(){
            this.visible=true
            this.title="添加栏目信息"
        },
        closeModalHandler(){
            this.visible=false
        },
        toDeleteHandler(){
            this,visible=true
            this.title="删除栏目信息"
        },
        toUpdateHandler(){
            this.visible=true
        }
    }
}
</script>
<style scoped>


</style>