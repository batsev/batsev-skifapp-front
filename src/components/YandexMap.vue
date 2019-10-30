<template>
  <div class="hello">
    <div id="map" style="height: 512px;width: 1024px; margin: 0 auto;"></div>
  </div>
</template>

<script>
import axios from "axios";
import ymaps from "ymaps";
export default {
  name: "YandexMap",
  data() {
    return {
      apiData: []
    };
  },
  created() {
    this.getFromApi();
  },
  methods: {
    async getFromApi() {
      await axios
        .get("api/get")
        .then(res => {
          this.apiData = res.data;
          this.mapInit();
        })
        .catch(function(error) {
          alert(error);
        });
    },
    async mapInit() {
      const maps = await ymaps.load();
      let myMap = new maps.Map("map", {
          center: [39, 57],
          zoom: 7
        }),
        objectManager = new maps.ObjectManager({
          clusterize: true
        });
      myMap.geoObjects.add(objectManager);
      objectManager.add(this.apiData);
    }
  }
};
</script>
