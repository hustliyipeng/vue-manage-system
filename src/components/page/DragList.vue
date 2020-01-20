<template>
    <section class="main">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-rank"></i> 拖拽组件</el-breadcrumb-item>
                <el-breadcrumb-item>拖拽排序</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="plugins-tips">
                我的待办事项
            </div>
            <div class="drag-box">
                <div class="drag-box-item">
                    <div class="item-title">todo</div>
                    <draggable v-model="todo" @remove="removeHandle" :options="dragOptions">
                        <transition-group tag="div" id="todo" class="item-ul">
                            <div v-for="(item,index) in todo" class="drag-list" :key="index" style="background: cadetblue;">
                                {{item.content}}
                            </div>
                            <div class="drag-list" key="add">
                                <el-input v-model="add_input" placeholder="新增待办事项..." @blur="add_todo"></el-input>
                            </div>
                        </transition-group>
                    </draggable>
                </div>
                <div class="drag-box-item">
                    <div class="item-title">doing</div>
                    <draggable v-model="doing" @remove="removeHandle" :options="dragOptions">
                        <transition-group tag="div" id="doing" class="item-ul">
                            <div v-for="(item,index) in doing" class="drag-list" :key="index" style="background: cornflowerblue;">
                                {{item.content}}
                            </div>
                        </transition-group>
                    </draggable>
                </div>
                <div class="drag-box-item">
                    <div class="item-title">done</div>
                    <draggable v-model="done" @remove="removeHandle" :options="dragOptions">
                        <transition-group tag="div" id="done" class="item-ul">
                            <div v-for="(item,index) in done" class="drag-list" :key="index" style="background: lightgray;">
                                {{item.content}}
                            </div>
                        </transition-group>
                    </draggable>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
    import draggable from 'vuedraggable'
    export default {
        name: 'draglist',
        data() {
            return {
                dragOptions:{
                    animation: 120,
                    scroll: true,
                    group: 'sortlist',
                    ghostClass: 'ghost-style'
                },
                todo: [],
                doing: [],
                done:[],
                add_input:null
            }
        },
        components:{
            draggable
        },
        created(){
            this.get_init_data()
        },
        
        methods: {
            get_init_data(){
                let data=JSON.parse(localStorage.getItem("todo_data"));
                console.log('data',data)
                this.todo=data.todo
                this.doing=data.doing
                this.done=data.done
            },
            set_todo_data(){
                let data={todo:this.todo,doing:this.doing,done:this.done}
                localStorage.setItem("todo_data",JSON.stringify(data));
            },
            removeHandle(event){
                console.log(event);
                this.$message.success(`从 ${event.from.id} 移动到 ${event.to.id} `);
                this.set_todo_data()
            },
            add_todo(){
                if(this.add_input){
                    console.log("add")
                    this.todo.push({content:this.add_input})
                    this.add_input=null
                    this.set_todo_data()
                }
            }
        }
    }

</script>

<style scoped>
    .drag-box{
        display: flex;
        user-select: none;
    }
    .drag-box-item {
        flex: 1;
        max-width: 420px;
        min-width: 300px;
        background-color: #eff1f5;
        margin-right: 16px;
        border-radius: 6px;
        border: 1px #e1e4e8 solid;
    }
    .item-title{
        padding: 8px 8px 8px 12px;
        font-size: 14px;
        line-height: 1.5;
        color: #24292e;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .el-icon-lx-roundadd{
        padding: 0 30px;
    }
    .item-ul{
        padding: 0 8px 8px;
        height: 560px;
        overflow-y: scroll;
    }
    .item-ul::-webkit-scrollbar{
        width: 0;
    }
    .drag-list {
        border: 1px #e1e4e8 solid;
        padding: 10px;
        margin: 5px 0 10px;
        list-style: none;
        background-color: #fff;
        border-radius: 6px;
        cursor: pointer;
        -webkit-transition: border .3s ease-in;
        transition: border .3s ease-in;
    }
    .drag-list:hover {
        border: 1px solid #20a0ff;
    }
    .drag-title {
        font-weight: 400;
        line-height: 25px;
        margin: 10px 0;
        font-size: 22px;
        color: #1f2f3d;
    }
    .ghost-style{
        display: block;
        color: transparent;
        border-style: dashed
    }
    .plugins-tips{
        text-align: center;
        font-size: 25px;
        font-weight: 600;
    }
</style>
