<template>
  <div id="PokeItem" v-if="datosPoke">
    <el-card :body-style="{ padding: '0px' }">
      <img
        :src="datosPoke.sprites.front_default"
        class="image"
        :style="{ backgroundColor: cambiarColor(datosPoke.types[0].type.name) }"
      />
      <div style="padding: 14px">
        <h2>{{ dato.name | capitalize }}</h2>
        <h3 :style="{ color: cambiarColor(datosPoke.types[0].type.name) }">
          {{ datosPoke.type | capitalize }}
        </h3>
        <h4>{{ datosPoke.weight + " lbs" }}</h4>
        <div class="bottom clearfix">
          <el-button type="text" @click="dialogVisible = true"
            ><i class="el-icon-view"></i
          ></el-button>

          <el-dialog
            title="Informacion básica"
            :visible.sync="dialogVisible"
            width="50%"
          >
            <h2>{{ dato.name | capitalize }}</h2>
            <h3 :style="{ color: cambiarColor(datosPoke.types[0].type.name) }">
              {{ datosPoke.type | capitalize }}
            </h3>
            <h2>Atributos del Pokemon</h2>
            <ul>
              <li v-for="(move, index) in moves" :key="index">
                {{ move.moves.name }}
              </li>
            </ul>
            <p>
              {{
                "La altura de este Pokemon es de " + datosPoke.height + " cm"
              }}
            </p>

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

  data() {
    return {
      datosPoke: {
        type: "",
        weight: Number,
        height: Number,
        moves: "",
      },
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
        case "poison":
          return "#a502b8";
        case "ground":
          return "#4d4d4d";
        case "electric":
          return "#f2ff00";
        case "normal":
          return "#1d232e";
      }
    },
  },
  mounted() {
    axios.get(this.dato.url).then((response) => {
      this.datosPoke = response.data;
      this.datosPoke.type = response.data.types[0].type.name;
      this.datosPoke.weight = response.data.weight;
      this.datosPoke.height = response.data.height;
      this.datosPoke.moves = response.data.moves[0].move.name;
      console.log(response);
    });
  },
  filters: {
    capitalize(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
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