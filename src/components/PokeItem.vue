<template>
  <div id="PokeItem" v-if="datosPoke">
    <el-card :body-style="{ padding: '0px' }">
      <img
        :src="datosPoke.sprites.front_default"
        class="image"
        :style="{ backgroundColor: cambiarColor(datosPoke.types[0].type.name) }"
      />
      <div style="padding: 14px">
        <span>{{ dato.name }}</span>
        <div class="bottom clearfix">
          <el-button type="text" @click="dialogVisible = true"
            ><i class="el-icon-view el-icon--right"></i
          ></el-button>

          <el-dialog
            title="Informacion básica"
            :visible.sync="dialogVisible"
            width="30%"
          >
            <span>Atributos del Pokemon</span>
            <span slot="footer" class="dialog-footer">
              <el-button type="danger" @click="dialogVisible = false"
                >Cerrar</el-button
              >
            </span>
          </el-dialog>
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "PokeItem",
  props: {
    dato: Object,
    index: Number,
  },
  el: "#example-1",
  data() {
    return {
      datosPoke: null,
      dialogVisible: false,
    };
  },
  methods: {
    cambiarColor(type) {
      switch (type) {
        case "grass":
          return "#34eb86";
        case "fire":
          return "#eb3453";
        case "water":
          return "#3474eb";
        case "bug":
          return "#b2bd55";
        case "normal":
          return "#1d232e";
      }
    },
  },
  mounted() {
    axios.get(this.dato.url).then((response) => {
      this.datosPoke = response.data;
    });
  },
};
</script>

<style>
.time {
  font-size: 13px;
  color: #999;
}

.bottom {
  margin-top: 13px;
  line-height: 12px;
}

.button {
  padding: 0;
  float: right;
}

.image {
  width: 100%;
  display: block;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both;
}
</style>