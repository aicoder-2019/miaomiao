<template>
  <div class="search_body">
    <div class="search_input">
      <div class="search_input_wrapper">
        <i class="iconfont icon-sousuo"></i>
        <input type="text" v-model="message" />
      </div>
    </div>
    <div class="search_result">
      <h3>电影/电视剧/综艺</h3>
      <ul>
        <li v-for="(item,index) in movieList" :key="index">
          <div class="img">
            <img :src="item.img | setWH('128.180')" />
          </div>
          <div class="info">
            <p>
              <span>{{item.nm}}</span>
              <span>{{item.sc}}</span>
            </p>
            <p>{{item.enm}}</p>
            <p>{{item.cat}}</p>
            <p>{{item.rt}}</p>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  name: "Search",
  data() {
    return {
      message: "",
      movieList: []
    };
  },
  methods: {
    cancelRequest() {
      if (typeof this.source === "function") {
        this.source("终止请求");
      }
    }
  },
  watch: {
    message(newVal) {
			// console.log(newVal);
			this.cancelRequest();
      this.axios
        .get("/api/searchList?cityId=10&kw=" + newVal, {
          cancelToken: new this.axios.CancelToken(c => {
            this.source = c;
          })
        })
        .then(res => {
          // console.log(res);
          var msg = res.data.msg;
          var movies = res.data.data.movies;
          if (msg && movies) {
            this.movieList = movies.list;
            console.log(this.movieList);
          }
        })
        .catch(err => {
          if (this.axios.isCancel(err)) {
            console.log("Rquest canceled", err.message); //请求如果被取消，这里是返回取消的message
          } else {
            //handle error
            console.log(err);
          }
        });
    }
  }
};
</script>
<style scoped>
</style>