<template>
  <div class="movie_body">
    <Loading v-if="isLoading"/>
    <Scroller v-else>
      <ul>
        <li v-for="(item,index) in comingList" :key="index">
          <div class="pic_show">
            <img :src="item.img | setWH('128.180')" />
          </div>
          <div class="info_list">
            <h2>
              {{item.nm}}
              <img v-if="item.version" src="../../assets/maxs.png" />
            </h2>
            <p>
              <span class="person">{{item.wish}}</span> 人想看
            </p>
            <p>主演: {{item.star}}</p>
            <p>{{item.rt}}上映</p>
          </div>
          <div class="btn_pre">预售</div>
        </li>
      </ul>
    </Scroller>
  </div>
</template>
<script>
export default {
  name: "ComingSoon",
  data() {
    return {
      comingList: [],
      isLoading:true,
      prevCityId: -1
    };
  },
  activated() {
    var cityId = this.$store.state.city.id;
    if (this.prevCityId === cityId) {
      return;
    }
    this.isLoading = true;
    console.log(111);
    this.axios.get("/api/movieComingList?cityId="+cityId).then(res => {
      console.log(res);
      var msg = res.data.msg;
      if (msg === "ok") {
        this.comingList = res.data.data.comingList;
        this.isLoading = false
        this.prevCityId = cityId;
      }
    });
  }
};
</script>
<style scoped>
</style>