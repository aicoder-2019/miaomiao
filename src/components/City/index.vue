<template>
  <div class="city_body">
    <div class="city_list">
      <div class="city_hot">
        <h2>热门城市</h2>
        <ul class="clearfix">
          <li v-for="(item,index) in hotList" :key="index">{{item.nm}}</li>
        </ul>
      </div>
      <div class="city_sort" ref="city_sort">
        <div v-for="(item,index) in cityList" :key="index">
          <h2>{{item.index}}</h2>
          <ul>
            <li v-for="(city,index) in item.list" :key="index">{{city.nm}}</li>
          </ul>
        </div>
      </div>
    </div>
    <div class="city_index">
      <ul>
        <li v-for="(item,index) in cityList" :key="index" @touchstart="handleToIndex(index)">{{item.index}}</li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  name: "City",
  data() {
    return {
      cityList: [],
      hotList: []
    };
  },
  mounted() {
    this.axios.get("/api/cityList").then(res => {
      // console.log(res);
      var msg = res.data.msg;
      if (msg === "ok") {
        var cities = res.data.data.cities;
        // [{ index: "A", list: [{ name: "深圳", id: 123 }] }];
        console.log(cities);
        var { cityList, hotList } = this.formatCityList(cities);
        this.cityList = cityList;
        this.hotList = hotList;
      }
    });
  },
  methods: {
    formatCityList(cities) {
      var cityList = [];
      var hotList = [];
      for (let i = 0; i < cities.length; i++) {
        if (cities[i].isHot === 1) {
          hotList.push(cities[i]);
        }
      }
      for (let i = 0; i < cities.length; i++) {
        var firstLetter = cities[i].py.substring(0, 1).toUpperCase();
        // console.log(firstLetter);
        if (toCom(firstLetter)) {
          cityList.push({
            index: firstLetter,
            list: [{ nm: cities[i].nm, id: cities[i].id }]
          });
        } else {
          for (let j = 0; j < cityList.length; j++) {
            if (cityList[j].index === firstLetter) {
              cityList[j].list.push({ nm: cities[i].nm, id: cities[i].id });
            }
          }
        }
      }
      cityList.sort((n1, n2) => {
        if (n1.index > n2.index) {
          return 1;
        } else if (n1.index < n2.index) {
          return -1;
        } else {
          return 0;
        }
      });
      function toCom(firstLetter) {
        for (let i = 0; i < cityList.length; i++) {
          if (cityList[i].index === firstLetter) {
            return false;
          }
        }
        return true;
      }
      console.log(cityList,hotList);
      return {
        cityList,
        hotList
      };
		},
		handleToIndex(index){
			var h2 =this.$refs.city_sort.getElementsByTagName('h2')
			this.$refs.city_sort.parentNode.scrollTop=h2[index].offsetTop
		}
  }
};
</script>
<style scoped>
</style>