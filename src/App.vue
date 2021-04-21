<template>
  <div id="app">
    <!-- <Login/> -->
    <h1 style="margin:auto;">产品数据小游戏</h1>
    <el-button style="margin-top:20px;" @click="start" type="danger" :disabled="noclick">{{buttontext}}</el-button>
    <h3>指标个数：{{indexnum}}</h3>
    <el-table
      :data="tableData"
      stripe
      style="width: 100%;margin:auto;">
      <el-table-column
        prop="index"
        label="指标">
      </el-table-column>
      <el-table-column
        prop="performance"
        label="数据表现">
      </el-table-column>
    </el-table>
    <h4>需要回答的问题：</h4>
    <h4>1.是否真实存在这样数据的产品？</h4>
    <h4>2.描述一下这个产品及理由？可以说一个现存的产品，可以想象一个产品，甚至可以想象一个有缺陷的产品。</h4>
    <h4>3.后续的产品计划？</h4>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {
  },
  methods:{
    randomNum:function(minNum, maxNum) {
      switch (arguments.length) {
        case 1:
          return parseInt(Math.random() * minNum + 1, 10);
        case 2:
          return parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10);
        default:
          return 0;
      }
    },
    next:function(){
        let ot = this;
        setTimeout(() => {
          ot.start();
        }, 100);
    },
    start:function(){
      let ot = this;
      if(this.buttontext==="开始游戏"){
        this.tableData = [];
        this.buttontext = "确定";
        this.loop = setInterval(() => {
          ot.indexnum = ot.randomNum(8,ot.indexes.length-5);
        }, 50);
      }else if(this.buttontext==="确定"){
        this.noclick = true;
        this.buttontext = "随机指标";
        clearInterval(this.loop);
        this.next();
      }else if(this.buttontext==="随机指标"){
        let newObj = JSON.parse(JSON.stringify(this.indexes));
        
        this.loop = setInterval(() => {
          if(ot.indexnum>0){
            let r = ot.randomNum(0,newObj.length-1);
            let out = newObj.splice(r,1);
            console.log(r)
            console.log(out)
            ot.tableData.push({"index":out[0]});
            ot.indexnum--;
          }else{
            ot.buttontext = "随机数据"
            clearInterval(ot.loop);
            ot.next()
          }
        }, 1000);
      }else if(this.buttontext==="随机数据"){
        this.loop = setInterval(() => {
          if(ot.indexnum<ot.tableData.length){
            let r = ot.randomNum(0,ot.performance.length-1);
            let out = ot.performance[r];
            let cu = ot.tableData[ot.indexnum];
            cu.performance = out;
            ot.tableData.splice(ot.indexnum,1,cu);
            // let tmp = ot.tableData;
            // ot.tableData = tmp;
            
            ot.indexnum++;
          }else{
            ot.noclick = false;
            ot.buttontext = "开始游戏"
            clearInterval(ot.loop);
          }
        }, 1000);
      }
    },
  },
  data:function(){
    return {
      noclick:false,
      tableData:[],
      buttontext:"开始游戏",
      indexnum:0,
      indexes:["新增","活跃","启动","单次使用时长","单日使用时长","次月留存(新增)","次日留存(新增)","7日留存(新增)","商店评分","次月留存(活跃)","次日留存(活跃)","7日留存(活跃)","只启动一次占比","NPS","平均功能使用深度","ARPU","LTV"],
      performance:["1(特别差)","2(略差)","3(平均)","4(略好)","5(特别好)"],
      };
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h4{
  text-align: left;
}
</style>
