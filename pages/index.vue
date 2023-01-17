<template>
  <v-row justify="center" align="center">
    <div id="viewer-container" />
  </v-row>
</template>

<script>
import { Color } from "three";
import { IfcViewerAPI } from "web-ifc-viewer";

import { AmbientLight, AxesHelper, DirectionalLight, GridHelper, PerspectiveCamera, Scene, WebGLRenderer } from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// import { IFCLoader } from "web-ifc-three/IFCLoader";

export default {

  name: 'IndexPage',
  data: () =>({

    viewer : null,
    ifcLoader: null,
  }),
  mounted() {

    const container = document.getElementById("viewer-container");
    this.viewer = new IfcViewerAPI({
      container,
      backgroundColor: new Color(0xffffff),
    });
    this.viewer.axes.setAxes();
    this.viewer.grid.setGrid();
    
    // this.viewer.IFC.loader = new IFCLoader();
    this.loadIfc("models/haus.ifc");

    window.onmousemove = () => this.viewer.IFC.selector.prePickIfcItem();

  },
  methods: {

    async loadIfc(url) {
      const model = await this.viewer.IFC.loadIfcUrl(url);
      this.viewer.shadowDropper.renderShadow(model.modelID);
    },

    onChange(change){

    }

  }
}
</script>
