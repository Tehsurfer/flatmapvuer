<template>
  <div id="app">
    <MultiFlatmapVuer ref="multi" :availableSpecies="availableSpecies" @resource-selected="FlatmapSelected" 
    @ready="FlatmapReady" :featureInfo="featureInfo" :searchable="searchable" :initial="initial"/>

    <div>
      <TooltipVuer placement="bottom" :visible="visible" :content="tContent" 
        :position="tStyle" :displayCloseButton="displayCloseButton" ref="tooltip" @onActionClick="onActionClick"/>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-alert, no-console */
import Vue from "vue";
import MultiFlatmapVuer from './components/MultiFlatmapVuer.vue'
import '@abi-software/maptooltip';
import '@abi-software/maptooltip/dist/maptooltip.css';
import {
  RadioButton,
  RadioGroup
} from 'element-ui';
import "./styles/purple/radio-button.css";
import "./styles/purple/radio-group.css";
import "./icons/mapicon-species-style.css";
Vue.use(RadioButton);
Vue.use(RadioGroup);

export default {
  name: 'app',
  methods: {
    FlatmapSelected: function(resource) {
      let tooltip = this.$refs.tooltip;
      this.$refs.multi.showPopup(resource.feature.id, tooltip.$refs.content.$vnode.elm);
    },
    FlatmapReady: function(component) {
      console.log(component.getLabels());
    },
    onActionClick: function(action) {
      console.log("onActionClick", action);
    }
  },
  data: function(){
    return {
      featureInfo: false,
      searchable: false,
      availableSpecies : {"Human":{taxo: "NCBITaxon:9606", iconClass:"icon-mapicon_human"},
        "Rat":{taxo: "NCBITaxon:10114", iconClass:"icon-mapicon_rat"} },
      tContent: {
        title: "Mapping of ICN Neurons in a 3D Rat Heart",
        description: "The distribution of neurons in the intrinsic cardiac nervous system (ICN) were mapped and visualized in a 3D reconstruction of a male rat heart.",
        actions: [
          {
            title: "View 3D scaffold",
            resource: "https://mapcore-bucket1.s3-us-west-2.amazonaws.com/others/29_Jan_2020/heartICN_metadata.json",
            type: "Scaffold"
          },
          {
            title: "View dataset",
            resource: "https://sparc.science/datasets/37?type=dataset",
            type: "URL"
          }
        ]
      },
      tStyle: {
        top: "200px",
        left: "200px",
        position: "absolute"
      },
      displayCloseButton: false,
      initial: "Rat",
      visible: false
    }
  },
  components: {
    MultiFlatmapVuer
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height:100%;
  width: 100%;
  position:absolute;
}

body {
  margin: 0px;
}

.el-tabs__content {
  height:100%;
}
</style>
