<template>
  <div id="Index">
    <h1>PokeVue</h1>
    <el-container>
      <el-row :offset="6" :span="15" :gutter="20">
        <el-col :span="18" :offset="3">
          <el-col :span="6" v-for="(item, index) in data" v-bind:key="index">
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
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=30").then((response) => {
      this.data = response.data.results;
      console.log(this.data);
    });
  },
};
</script>

<style>
</style>