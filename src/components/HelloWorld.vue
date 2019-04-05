<template>
  <div class="hello">
      
  </div>
</template>

<script>

export default {
  name: 'HelloWorld',
  data () {
    return {
      arr :[1,2,3,4]
    }
  },
  methods: {
    
  },
  mounted(){
    // delete arr[3]
    // console.log(this.arr,"返回删除后是数组")

    // 第一种写法
    // var b = this.arr.filter(item => {return item !== 4})
    // console.log(b)

    var b = this.arr.some((item,index,array) =>{
      return item === 4 
      })
      console.log(b)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .a{
    background:pink;
  }
  .b{
    background:yellow;
  }
  .vertical-center-modal{
    display: flex;
    align-items: center;
    justify-content: center;
        
  }
  .ivu-modal{
    top: 0;
  }
</style>
