<template>
  <div>
    <home-header></home-header>
    <home-swiper :list="swiperList"></home-swiper>
    <home-icons :list="iconList"></home-icons>
    <home-recommend :list="recommendList"></home-recommend>
    <home-weekend :list="weekendList"></home-weekend>
  </div>
</template>

<script>
import HomeHeader from './components/Header'
import HomeSwiper from './components/Swiper'
import HomeIcons from './components/Icons'
import HomeRecommend from './components/Recommend'
import HomeWeekend from './components/Weekend'
import {mapState} from 'vuex'
import axios from 'axios'
  export default {
    name:"Home", //组件命名
    components:{
      HomeHeader,
      HomeSwiper,
      HomeIcons,
      HomeRecommend,
      HomeWeekend
    },
    data (){
      return{
        lastCity:'',
        iconList:[],
        swiperList:[],
        recommendList:[],
        weekendList:[],
      }
    },
    computed:{
      ...mapState(['city'])
    },
    methods:{
      getHomeInfo(){
       // axios.get('/api/index.json',method:'GET',{params:{id:"01"}});
        axios.get('/api/index.json?city='+this.city)
          .then(this.getHomeInfoSucc)
      },
      getHomeInfoSucc(resp){
        resp=resp.data;
        if(resp.ret && resp.data){
          resp=resp.data;
          this.iconList=resp.iconList;
          this.swiperList=resp.swiperList;
          this.recommendList=resp.recommendList;
          this.weekendList=resp.weekendList;
        }
        console.log("resp:"+resp);
      }
    },
    mounted (){
      console.log("mounted");
      this.lastCity=this.city;
      this.getHomeInfo();
    },
    //页面重新显示的时候会执行
    activated (){
      if(this.label != this.city){
        this.lastCity=this.city;
        this.getHomeInfo();
      }
    }
  }
</script>

<style>

</style>
