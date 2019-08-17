<template>
  <div class="cinema_body">
    <ul>
      <li v-for="(item,index) in cinemaList" :key="index">
        <div>
          <span>{{item.nm}}</span>
          <span class="q">
            <span class="price">{{item.sellPrice}}</span> 元起
          </span>
        </div>
        <div class="address">
          <span>{{item.addr}}</span>
          <span>{{item.distance}}</span>
        </div>
        <div class="card">
          <div v-for="(num,key) in item.tag" v-if="num===1" :key="key" :class="key | classCard">{{key | formatCard(key)}}</div>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "CiList",
  data() {
    return {
      cinemaList: []
    };
  },
  mounted() {
    this.axios.get("/api/cinemaList?cityId=10").then(res => {
      // console.log(res);
      var msg = res.data.msg;
      if (msg === "ok") {
        this.cinemaList = res.data.data.cinemas;
        console.log(this.cinemaList);
      }
    });
  },
  filters: {
    formatCard(key) {
      var card = [
        { key: "allowRefund", value: "改签" },
        { key: "endorse", value: "退" },
        { key: "sell", value: "折扣卡" },
        { key: "snack", value: "小吃" }
      ];
      for (let i = 0; i < card.length; i++) {
        if (card[i].key === key) {
          return card[i].value;
        }
      }
      // return "";
		},
		classCard(key) {
      var card = [
        { key: "allowRefund", value: "bl" },
        { key: "endorse", value: "bl" },
        { key: "sell", value: "or" },
        { key: "snack", value: "or" }
      ];
      for (let i = 0; i < card.length; i++) {
        if (card[i].key === key) {
          return card[i].value;
        }
      }
      // return "";
    }
  }
};
</script>
<style scoped>
</style>