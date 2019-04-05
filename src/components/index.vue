<template>
    <section class="index">
        <!-- <p>¹ŠÄÜÕýÔÚ¿ª·¢ÖÐ£¬ŸŽÇëÆÚŽý</p> -->
        <!--{{addressOptions}}-->
        <button></button>
        <el-button type="primary" @click="handleCheckAll(true)">È«Ñ¡</el-button>
        <el-button type="warning" @click="handleCheckAll(false)">ÖØÖÃ</el-button>
        <button></button>
        <ItemList v-for="(item,index) in addressOptions" :key="index" :item="item" />
        <el-button type="primary" @click="saveBtn">±£Žæ</el-button>
        <el-button type="warning" @click="cancelBtn">È¡Ïû</el-button>
    </section>
</template>

<script>

// import axios from "axios";
import ItemList from "./ItemList";
import bus from "@/utils/bus";
import data from "../../static/data.json";
export default {
    name: "index",
    data() {
        return {
            addressOptions: null,
            // status:'',
            checkoption: []
        };
    },
    components: {
        ItemList
    },
    methods: {
        handleCheckAll(checkAll) {
            this.recursion(this.addressOptions, checkAll);
        },
        recursion(data, checkAll) {
            data.map(value => {
                value.checkAll = checkAll;
                if (value.subMenuList) {
                    this.recursion(value.subMenuList, checkAll);
                } else {
                    value.permissionPackageList.map(v => {
                        v.checkAll = value.checkAll;
                    });
                }
            });
        },
        active(value, checkState) {
            if (value.subMenuList) {
                if (checkState.length == value.subMenuList.length) {
                    value.checkAll = true;
                    value.isIndeterminate = false;
                } else if (checkState.length != 0) {
                    value.checkAll = false;
                    value.isIndeterminate = true;
                } else if (checkState.length == 0) {
                    value.checkAll = false;
                    value.isIndeterminate = false;
                }
            } else {
                if (value.checkedOption.length == value.permissionPackageList.length) {
                    value.checkAll = true;
                    value.isIndeterminate = false;
                } else if (value.checkedOption.length != 0) {
                    value.checkAll = false;
                    value.isIndeterminate = true;
                } else if (value.checkedOption.length == 0) {
                    value.checkAll = false;
                    value.isIndeterminate = false;
                }
            }
        },

        checkStatus(data) {
            data.map(value => {
                var checkState = [];
                if (value.subMenuList) {
                    let subMenuList = value.subMenuList;
                    subMenuList.map(val => {
                        this.checkStatus(value.subMenuList);
                        if (val.checkAll == true) {
                            checkState.push(val.checkAll);
                        }
                        this.active(value, checkState);
                    });
                } else {
                    this.active(value);
                }
            });
        },
        init() {
            // this.$http({
            //     url: "./static/data.json",
            //     methods: "post"
            // }).then(res => {
            //     if (res && res.data) {
                    
            //     }
            // });
            console.log(data);
            this.addressOptions = data.data.menu;
            this.checkStatus(this.addressOptions);
        },
        saveCheck(data) {
            if (data.checkAll == true) {
                if (data.permissionPackageList) {
                    data.permissionPackageList.map((va, ins) => {
                        this.checkoption.push(va.id);
                    });
                } else if (data.subMenuList) {
                    data.subMenuList.map((val, index) => {
                        this.saveCheck(val);
                    });
                } else {
                    this.checkoption.push(data.id);
                }
            } else {
                if (data.permissionPackageList) {
                    data.permissionPackageList.map((va, ins) => {
                        this.checkoption = this.checkoption.filter(item => item != va.id);
                    });
                } else if (data.subMenuList) {
                    data.subMenuList.map((val, index) => {
                        this.saveCheck(val);
                    });
                } else {
                    this.checkoption = this.checkoption.filter(item => item != data.id);
                }
            }
        },

        saveBtn() {
            // console.log(this.checkoption, "Ñ¡ÖÐµÄÖµ");
        },
        cancelBtn() {
            alert("È¡Ïû");
        }
    },
    watch:{

    },
    mounted(){
        this.init();
        bus.$on("childByValue", e => {
            this.saveCheck(e);
            console.log(e,111111111)
            if(e.permissionPackageList || e.subMenuList){
                console.log(e,"ÓÐ²ãŒ¶")
            }else{
                console.log("×îµ×²ã")
            }
            // console.log(this.addressOptions,11111111)
            this.addressOptions.map(item => {
                
            })
        });
        
    }
};
</script>

<style rel="stylesheet/css">
.index > .checkbox > .el-checkbox-group > .labelbox {
    width: 100%;
    height: 40px;
    background: #f5f6fa;
    line-height: 40px;
    border-bottom: 1px dashed #a6b7bf;
}
.index > .checkbox > .el-checkbox-group > .labelbox + div {
    margin-left: 20px;
}
.index > .checkbox .el-checkbox-group .checkbox {
    margin-left: 20px;
}
.index > .checkbox .checkbox {
    margin-left: 20px;
}
.index > .checkbox .checkbox .labelbox ~ div {
    margin-left: 20px;
    width: 14%;
    display: inline-block;
}
</style>