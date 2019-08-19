<template>
  <div id="detailContainer" class="slide-enter-active">
    <Header title="影片详情">
      <i class="iconfont icon-right" @touchstart="handleToBack"></i>
    </Header>
    <Loading v-if="isLoading"/>
    <div v-else id="content" class="contentDetail">
      <div class="detail_list">
        <div class="detail_list_bg"></div>
        <div class="detail_list_filter"></div>
        <div class="detail_list_content">
          <div class="detail_list_img">
            <img :src="detailMovie.albumImg || detailMovie.img | setWH('148.208')" alt />
          </div>
          <div class="detail_list_info">
            <h2>{{detailMovie.nm}}</h2>
            <p>{{detailMovie.enm}}</p>
            <p>{{detailMovie.sc}}</p>
            <p>{{detailMovie.cat}}</p>
            <p>{{detailMovie.src}} / {{detailMovie.dur}}分钟</p>
            <p>{{detailMovie.pubDesc}}</p>
          </div>
        </div>
      </div>
      <div class="detail_intro">
        <p>{{detailMovie.dra}}</p>
      </div>
      <div class="detail_player swiper-container" ref="detail_player">
        <ul class="swiper-wrapper">
          <li v-for="(item,index) in detailMovie.photos" :key="index" class="swiper-slide">
            <div>
              <img :src="item | setWH('140.127')" alt />
            </div>
            <!-- <p>陈建斌</p>
            <p>马先勇</p> -->
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import Header from "../../components/Header";
export default {
  name: "Detail",
  data() {
    return {
      detailMovie:{},
      isLoading:true
    };
  },
  components: {
    Header
  },
  props: ["movieId"],
  methods: {
    handleToBack() {
      this.$router.back();
    }
  },
  mounted() {
    // console.log(this.movieId);
    this.axios('/api/detailmovie?movieId='+this.movieId).then(res=>{
      var msg = res.data.msg
      if(msg ==='ok'){
        this.detailMovie = res.data.data.detailMovie
        this.isLoading = false
        console.log(this.detailMovie);
        this.$nextTick(()=>{
          new Swiper(this.$refs.detail_player,{
            slidesPerView:'auto',
            freeMode:true,
            freeModeSticky:true
          })
        })
      }
    })
  }
};
</script>
<style scoped>
#detailContainer {
  position: absolute;
  left: 0;
  top: 0;
  z-index: 100;
  width: 100%;
  min-height: 100%;
  background: #fff;
}
#detailContainer.slide-enter-active {
  animation: slideMove .3s;
}
@keyframes slideMove {
  0% {
    transform: translateX(100%);
  }
  100% {
    transform: translateX(0%);
  }
}
</style>