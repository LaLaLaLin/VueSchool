<template>
    <div>   
    <Row>
      <h3>用户设置</h3>
         <Table border  ref="selection" :columns="columns4" :data="historyData"></Table>
         <Page :total="count" :page-size="tiao" :current="ye" show-sizer   show-total @on-change="Yebian" @on-page-size-change="TiaoBian" style="margin-top:10px;"/>
     <Modal
                v-model="modal2"
                title="修改用户信息"
                @on-ok="XiuTi"
              >
                <Form ref="Dui" :model="Dui" :label-width="100">
                        <FormItem label="用户名：">
                            <Input type="text" v-model="Dui.userName" clearable />
                        </FormItem>
                         <FormItem label="账号：">
                            <Input type="text" v-model="Dui.userCid" clearable />
                        </FormItem>
                         <FormItem label="密码：">
                            <Input type="text" v-model="Dui.password" clearable />
                        </FormItem>
                </Form>
            </Modal>
    </Row>
        
    </div>
</template>
<script>
export default {
    data(){
        return{
             Dui:{
                id:'',
                userName:'',    
                userCid:'',
                password:'',
            },//进行修改角色

            modal1:false,
            modal2:false,
            columns4: [
                    {
                        type: 'selection',
                        width: 60,
                        align: 'center'
                    },
                    {
                        title: '姓名',
                        key: 'userName'
                    },
                    {
                        title: '账号',
                        key:'userCid',
                    },{
                        title:'密码',
                        key:'password',
                    }
                    ,{
                    title:'操作',
                    render:(h,obj)=>{
                        let btn1 = {
                            // 属性
                            props:{
                                type:'primary',
                            },
                            style:{
                                marginRight:'8px'
                            },
                            // 事件
                            on:{
                                click:()=>{
                                    //进入用户的角色中
                                    this.$store.state.Usertoken.Dan=obj.row;
                                    this.$router.push('/UserRole');
                                }
                            }
                        };
                        let btn2 = {
                            // 属性
                            props:{
                                type:'success',
                            },
                            style:{
                                marginRight:'8px'
                            },
                            // 事件
                            on:{
                                click:()=>{
                                     this.Dui.userName=obj.row.userName;
                                     this.Dui.userCid=obj.row.userCid;
                                     this.Dui.password=obj.row.password;
                                     this.Dui.id=obj.row.id,
                                     this.modal2=true;
                                }
                            }
                        };
                         let btn3 = {
                            // 属性
                            props:{
                                type:'error',
                            },
                            // 事件
                            on:{
                                click:()=>{
                                   this.Chu('UserDeleteAction',obj.row.id);
                                }
                            }
                        };
                        let arry = [
                            h('Button',btn1,'用户角色'),
                            h('Button',btn2,'修改'),
                            h('Button',btn3,'删除'),
                        ]
                        return h('div',arry);
                    }
                    }
                ],
                zong:[],//总数据
                historyData:[],//显示的数据
                ye:2,//当前页数
                count:50,//总页数
                tiao:5,//每页条数

        
        }
    },
     created(){
          this.Chu('SystemListUser');
    },
    methods:{
        Chu(name){
            this.$store.dispatch(name).then(res=>{
                            if(res!=null){
                                this.zong=res;
                                this.count=this.zong.length;
                                if(this.count<this.tiao){
                                    this.historyData=this.zong;
                                }else{
                                    this.historyData=this.zong.slice(0,this.tiao);
                                }
                            }else
                            {
                                alert('没有数据');
                            }
                        })
            },
        Chu(name,dui){
            this.$store.dispatch(name,dui).then(res=>{
                            if(res!=null){
                                this.zong=res;
                                this.count=this.zong.length;
                                if(this.count<this.tiao){
                                    this.historyData=this.zong;
                                }else{
                                    this.historyData=this.zong.slice(0,this.tiao);
                                }
                            }else
                            {
                                alert('没有数据');
                            }
                        })
            },
            Yebian(s){
                let kai=(s-1)*this.tiao;//开始
                let jie=s*this.tiao;//结束
                this.historyData=this.zong.slice(kai,jie);
            },
            TiaoBian(s){ 
                this.historyData=this.zong.slice(0,s);
            },
            //查询角色
            Jue(s){
                this.modal1=true;
                console.log(s);
            },
            //修改进行提交
            XiuTi(){
                console.log(this.Dui);
                this.Chu('UserGaiAction',this.Dui);
            }
        
            
    }
}
</script>
