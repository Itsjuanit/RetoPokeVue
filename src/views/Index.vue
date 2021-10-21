<template>
  <div id="Index">
    <img src="../assets/pokee.png" alt="" />
    <el-container>
      <el-row :offset="6" :span="15" :gutter="20">
        <el-col :span="18" :offset="3">
          <el-col
            :span="6"
            v-for="(item, index) in data"
            v-bind:key="index"
            class="newCard"
          >
            <PokeItem :dato="item" :index="index" />
          </el-col>
        </el-col>
      </el-row>
    </el-container>
  </div>
</template>

<script>
import axios from "axios";
import PokeItem from "../components/PokeItem";

export default {
  name: "Index",
  components: {
    PokeItem,
  },
  data() {
    return {
      data: [],
    };
  },
  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=40").then((response) => {
      this.data = response.data.results;
      //console.log(this.data);
    });
  },
};
</script>

<style>
.newCard {
  margin-bottom: 20px;
}
</style>