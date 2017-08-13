<template>
  <div id="app">
    <div class="content">
      <scroll :isProvince=true class="item" @provinceChoose="provinceChoose"></scroll>
      <scroll :isCity=true class="item" :provinceName="province" @cityChoose="cityChoose"></scroll>
      <scroll :isTown=true class="item" :provinceName="province" :cityName="city"></scroll>
    </div>
  </div>
</template>

<script>
import Scroll from './components/scroll'
export default {
  name: 'app',
  components: {
    Scroll
  },
  data(){
    return {
      province:'',
      city:''
    }
  },
  methods:{
    provinceChoose(item){
      this.province=item
    },
    cityChoose(item){
      this.city=item
    }
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus" scoped>
  .title
    height 70px
    font-size 50px
    line-height 70px
    text-align center
  .content
    display flex
    overflow hidden
    .item
      flex 1
</style>
