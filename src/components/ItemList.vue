<template>
    <section class="checkbox">
        <template>
            <el-checkbox-group v-model="item.checkAll">
                <div class="labelbox">
                    <el-checkbox :indeterminate="item.isIndeterminate" v-model="item.checkAll"  @change="navigationBtn(item)" :label="item.id" >{{item.name}}</el-checkbox>
                </div>
                <template v-for="v in item.permissionPackageList">
                    <el-checkbox-group v-model="v.checkAll" @change="permissionsButton(v,item)">
                        <el-checkbox v-model="v.checkAll"  :label="v.id">{{v.name}}</el-checkbox>
                    </el-checkbox-group>
                </template>
                <item-list v-for="(child,index) in item.subMenuList" :key="index" :item="child"></item-list>
            </el-checkbox-group>
            <button></button>
        </template>
    </section>
</template>

<script>

    import bus from "@/utils/bus";
    export default {
        name: "itemList",
        props: {
            item: {
                type: Object,
                required: true
            }
        },
        data(){
            return {
                checkList:'',
            }
        },
        methods:{
            active(value){
                var kai = false;
                var ab = 0;
                // value.subMenuList.forEach((item,index)=>{
                //     if(!item.checkAll){
                //         kai = true;
                //     }
                //     if(item.checkedOption.length==0){
                //         ab++;
                //     }
                // });

                // if(kai){
                //     value.isIndeterminate = true;
                //     value.checkAll = false;
                // }else{
                //     value.isIndeterminate = false;
                //     value.checkAll = true;
                // }
                // if(ab == value.subMenuList.length){
                //     value.isIndeterminate = false;
                //     value.checkAll = false;
                //     // console.log(value,"value")
                //     // console.log(ab,value.subMenuList.length)
                // }
            },
            navigationBtn(value){
                // µã»÷¹ŽÑ¡žžÇ× ÇÒ ËùÓÐ¶ù×Ó
                console.log(value)
                this.recursion(value)
                bus.$emit('childByValue', value)
            },
            recursion(data){
                if(data.subMenuList){
                    data.subMenuList.map((val,index)=>{
                        val.checkAll = data.checkAll;
                        if(val.subMenuList){
                            this.recursion(val)

                        }else{
                            val.permissionPackageList.map((va,ins)=>{
                                va.checkAll = val.checkAll
                            })
                        }
                    })

                } else{
                    data.permissionPackageList.map((va,ins)=>{
                        va.checkAll = data.checkAll
                    })
                }
            },
            permissionsButton(value,item){
                console.log(value,item)
                // console.log(value,'×ÓŒ¶')
                // console.log(item.name,item,'žžŒ¶')
                var checkState = [];
                item.permissionPackageList.map(child =>{
                    // console.log(child.checkAll,"×ŽÌ¬")
                    if(child.checkAll == true){
                        checkState.push(child.checkAll)
                    }
                    
                })
                console.log(checkState,item.permissionPackageList.length)
                if(checkState.length == item.permissionPackageList.length){
                    item.checkAll = true;
                    item.isIndeterminate = false;
                }else if(checkState.length != 0){
                    item.checkAll = false;
                    item.isIndeterminate = true;
                }else if(checkState.length == 0){
                    item.checkAll = false;
                    item.isIndeterminate = false;
                }
                bus.$emit('childByValue', value)
            },
        },
        watch:{

        },
        mounted(){
        }
    }
</script>

<style rel="stylesheet/css">
.checkbox{
    padding: 25px 0;
    /* // .labelbox{
    //     width: 100%;
    //     height: 40px;
    //     background: #f5f6fa;
    //     line-height: 40px;
    //     border-bottom: 1px dashed #a6b7bf;
    // } */
}
</style>