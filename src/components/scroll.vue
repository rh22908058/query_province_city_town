<template>
<div  class="warpper" ref='wrapper'>
  <ul>
    <li v-for="item in data" @click="choose(item)" class="item">{{item}}</li>
  </ul>
</div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
export default {
  data () {
    return {
      data:[],
      Bs:{}
    }
  },
  props:{
    //定义组件的类型
    isProvince:{
      type:Boolean,
      default:false
    },
    isCity:{
      type:Boolean,
      default:false
    },
    isTown:{
      type:Boolean,
      default:false
    },
    //在对应类型下接收省名或省市名，作为请求参数
    provinceName:{
      type:String
    },
    cityName:{
      type:String
    }
  },
  created(){
    //如果是获取全部省份
    if(this.isProvince){
      axios.get('http://localhost:3000/getData').then(res=>{
        if(!res.data.err){
          this.data=res.data.data
          console.log(this.data)
           this.$nextTick(function(){
              this._initScroll()
          })
        }
      })
    }   
  },
  methods:{
    //选择了省市区，给父组件派发事件传递点击项
    choose(item){
      if(this.isProvince){
        this.$emit('provinceChoose',item)
      }else if(this.isCity){
        this.$emit('cityChoose',item)
      }else{
        this.$emit('townChoose',item)
      }
    },
    //判断对象是否为空
    isEmpty(obj){
      let count=0
      for(let prop in obj){
        count++
      }
      if(count==0){
        return true
      }else{
        return false
      }
    },
    //创建滚动组件，使三个scroll分别产生滚动效果
    _initScroll(){
      if(this.isEmpty(this.Bs)){
        this.Bs=new BScroll(this.$refs.wrapper,{
            //监听click,因为子组件cartcontrol需要添加点击事件
            click:true
        }) 
      }else{
        this.Bs.refresh()
      }
    }
  },
  watch:{
    //省份名provinceName变化，发送get请求获取所有citys并渲染
    'provinceName':function(newVal){
      console.log('provinceName change')
      axios.get(`http://localhost:3000/getData/${newVal}`).then(res=>{
        if(!res.data.err){
          if(this.isCity){
            this.data=res.data.data
            console.log(this.data)
          }else if(this.isTown){
            this.data=[]
          }
        }
        this.$nextTick(function(){
            this._initScroll()
        })
      })
    },
    //市名cityName变化，发送get请求获取所有towns并渲染
    'cityName':function(newVal){
      console.log('cityName change')
      axios.get(`http://localhost:3000/getData/${this.provinceName}/${newVal}`).then(res=>{
        if(!res.data.err){
          this.data=res.data.data
          console.log(this.data)
          this.$nextTick(function(){
              this._initScroll()
          })
        }
      })
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
.warpper
  text-align center
  font-size 24px
  padding 0
  margin 0
  overflow hidden
  /*设置warpper容器为视口的100%高度*/
  height 100vh
  .item
    list-style none
    height 120px
    line-height 120px
    border-bottom 1px solid #666
    margin 0 20px
    font-size 24px
    cursor pointer
</style>
