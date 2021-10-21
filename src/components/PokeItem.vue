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
            <span
              :style="{ color: cambiarColor(datosPoke.types[0].type.name) }"
            >
              {{ datosPoke.type | capitalize }}
            </span>
            <p>
              {{
                "La altura de este Pokemon es de " + datosPoke.height + " cm"
              }}
            </p>
            <p>
              {{ "Este Pokemon tiene " + datosPoke.moves.length + " ataques" }}
            </p>
            <p>
              {{ "El ID de este Pokemon en la Pokedex es el " + datosPoke.id }}
            </p>
            <p>
              {{ "El XP de este Pokemon es " + datosPoke.base_experience }}
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
        type: [],
        weight: Number,
        height: Number,
        id: Number,
        base_experience: Number,
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
        case "fairy":
          return "#9c9100";
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
      this.datosPoke.id = response.data.id;
      this.datosPoke.base_experience = response.data.base_experience;
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
.circle {
  display: flex;
  justify-content: center;
  max-width: 50px;
  border-radius: 10px;
  padding: 3px;
  border: 1px solid #212121;
}
</style>