<template>
  <div class="hello">
     <table border="1">
      <tr>
        <td style="width:10%">颜色/尺码</td>
        <td>M</td>
        <td>L</td>
        <td>S</td>
        <td>小计</td>
      </tr>
      <tr v-for='(item,index) in DataJson'>
        <td>{{item.color}}</td>
        <td><input type="number" :placeholder="item.M" v-model="MNumber[index]" @input="keepStore(item.M,index,'M')"></td>
        <td><input type="number" :placeholder="item.L" v-model="LNumber[index]" @input="keepStore(item.M,index,'L')"></td>
        <td><input type="number" :placeholder="item.S" v-model="SNumber[index]" @input="keepStore(item.M,index,'S')"></td>
        <td>{{getAllcount(index,item)}}</td>
      </tr>
      <tr>
        <td colspan="5">
          总件数：{{allNumber}},总金额：￥{{allNumber*100}}.
            <button :disabled="disabled">提交订单</button>
        </td>
            
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      MNumber:[],
      LNumber:[],
      SNumber:[],
      DataJson:[],
      disabled:true
    }
  },
   created:function() {
    let that=this
    this.$http.get('../../static/test.json').then(function(res){
      console.log(res)
      that.DataJson=res.data;
    })
  },
  methods:{
    getAllcount: function(index,item){
      if(this.keepStore){
        let MNumber = Number(this.MNumber[index])
        let LNumber = Number(this.LNumber[index])
        let SNumber = Number(this.SNumber[index])
        if(!this.MNumber[index]){
            MNumber=0
        }
        if(!this.LNumber[index]){
            LNumber=0
        }
        if(!this.SNumber[index]){
            SNumber=0
        }
        if(MNumber+LNumber+SNumber){
          this.disabled=false
        }else{
          this.disabled=true
        }
        return MNumber+LNumber+SNumber
      }   
    },
    keepStore:function(v,i,flag) {
      if(Number(event.target.value)>v){
        alert('库存不够')
        event.target.value=0
        event.target.focus()
        if(flag=="M"){
          this.MNumber[i]=0
        }else if(flag=="L"){
          this.LNumber[i]=0
        }else{
          this.SNumber[i]=0
        } 
        return false
      }else{
        return true
      }
    }
  },
  computed:{
    allNumber:function(){
      let all=0;
      for(let i in this.DataJson){
        all+=this.getAllcount(i)
      }
      return all
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table{
    width: 80%;
    margin: 0 auto;
    display: table;
}
td{

}

</style>
