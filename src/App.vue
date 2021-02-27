<template>
  <div id="app">
    <label for="options">Choose a variety of olives:</label><br>
    <select name="options" id="options">
      <option value="black" v-on:click="createGraph">black</option>
      <option value="green" v-on:click="createGraph">green</option>
    </select>
    <Plotly style="width:600px;height:250px;" :data="data" :layout="layout" :display-mode-bar="false"></Plotly>
  </div>
</template>

<script>
import olivetrees from "../public/olive_stats.json";
import { Plotly } from 'vue-plotly';

export default {
  name: 'App',
  components: {
    Plotly
  },
  data() {
    return {
      mydata: olivetrees,
      data:[{
        x: [],
        y: [],
        type:"bar"
      }],
      layout:{
        title: "Graph: Olive Trees Distribution"
      },
    };
  },
  methods: {
    createGraph: function() {

      if (document.getElementById("options").value=='black'){

        var first = this.mydata.filter(el => el.color=='black');

        // filter by each olive type:
        var typeA = first.filter(el => el.oliveType=='Kalamon');
        var typeB = first.filter(el => el.oliveType=='Kothreiki');

        // how many trees of this color & olive type there are:
        var sumA = typeA.length;
        var sumB = typeB.length;

        // use those data to create the chart:
        this.data[0].x = [];      // empty both arrays in case they had data from the other olive type
        this.data[0].y = [];
        this.data[0].x.push('Kalamon','Kothreiki');
        this.data[0].y.push(sumA,sumB);
      }
      else if (document.getElementById("options").value=='green'){
        var sec = this.mydata.filter(el => el.color=='green');
        var sumSec = sec.length;
        this.data[0].x = [];      // again, emptying both arrays
        this.data[0].y = [];
        this.data[0].x.push('Cerignola');
        this.data[0].y.push(sumSec);
      }
    }
  },
  mounted: function(){
    document.getElementById("options").value='black';   //this is needed to display a graph when the page firstly loads. otherwise, an empty graph would be created
    this.createGraph();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>