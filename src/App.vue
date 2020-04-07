<template>
  <div id="app" style="widht:100vw;height:100%;" @drop="createNewJson($event.dataTransfer.files)">
    <Header id="header" @openMenu="expandSettings = true" />
    <transition name="menu">
      <Settings v-if="expandSettings" @hide="expandSettings = false" style="z-index:99;" />
    </transition>
    <div class="body" style="position:relative">
      <ul class="list-group">
        <li
          class="list-group-item list-group-item-primary btn btn-primary text-left"
          @click="update()"
        >
          Update All configs
          <strong class="float-right">{{allConfigsLastUpdated}}</strong>
        </li>
        <li
          class="list-group-item list-group-item-success btn btn-success text-left"
          style="width:370px"
          @click="update('cdrCallTypes')"
        >
          Update cdrCallTypes
          <strong class="float-right">{{cdrCallTypesLastUpdated}}</strong>
        </li>
        <li
          class="list-group-item list-group-item-success btn btn-success text-left"
          @click="update('cdrDestinationInfo')"
        >
          Update cdrDestinationInfo
          <strong class="float-right">{{cdrDestinationInfoLastUpdated}}</strong>
        </li>
        <li
          class="list-group-item list-group-item-success btn btn-success text-left"
          @click="update('sdpProductDetails')"
        >
          Update sdpProductDetails
          <strong class="float-right">{{sdpProductDetailsLastUpdated}}</strong>
        </li>
        <li
          class="list-group-item list-group-item-success btn btn-success text-left"
          @click="update('streamerConfigurations')"
        >
          Update streamerConfigurations
          <strong class="float-right">{{streamerConfigurationsLastUpdated}}</strong>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import Header from "./components/header";
import Settings from "./components/settings";
export default {
  data() {
    return {
      expandSettings: false,
      allConfigsLastUpdated: "",
      cdrCallTypesLastUpdated: "",
      cdrDestinationInfoLastUpdated: "",
      sdpProductDetailsLastUpdated: "",
      streamerConfigurationsLastUpdated: ""
    };
  },
  components: {
    Header,
    Settings
  },
  async created() {},
  methods: {
    update(conf) {
      let url = "http://t1vl604:8180/configreset?configs=" + conf
      let c = conf ? conf + "LastUpdated" : "allConfigsLastUpdated";
      fetch(url,{cache: "no-store"})
        .then(r => r.text())
        .then(() => {
          let t = " (" + new Date().format("mm/dd hh:MM TT") + ")";
          if (!conf) {
            [
              "cdrCallTypesLastUpdated",
              "cdrDestinationInfoLastUpdated",
              "sdpProductDetailsLastUpdated",
              "streamerConfigurationsLastUpdated"
            ].forEach(k => (this[k] = t));
          }
          this[c] = t;
        })
        .catch((e) => (console.log(e,this[c]), (this[c] = " (Error!!!)")));
    }
  },
  watch: {},
  computed: {}
};
</script>
<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#header {
  height: 30px;
}
.body {
  position: relative;
  background-color: white;
  display: flex;
  overflow: hidden;
  padding: 10px;
  height: calc(100% - 30px);
}
.clickable {
  user-select: none;
  cursor: pointer;
}
</style>
