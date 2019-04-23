<template>
  <div id="app">
    <!-- <h3>Coloured grids</h3>
    <input type="text" id="text-input" value="5000">
    <div id="text-explain">...takes numbers between 1 and 10k</div>
    <div id="container"></div>
    <button @click="getState">Hello state</button> -->
    <myCanvas></myCanvas>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import myCanvas from './components/myCanvas.vue';
import state from 'state';
import * as d3 from "d3";

export default {
  name: 'app',
  data () {
    return {
      width: 750,
			height: 400,
	    nodes: [],
		  value: 5000,
		  colourScale: null,
      canvas: null,
      context: null,
      customBase: null,
		  custom: null,
      groupSpacing: 4,
		  cellSpacing: 2,
		  cellSize: Math.floor((this.width - 11 * this.groupSpacing) / 100) - this.cellSpacing,
      person: {
        greet: function () { return "Hello."; }
      }
    }
  },
  components: {
    HelloWorld,
    myCanvas
  },
  methods: {
    getState () {
      console.log(this.person.state('-> Casual'))
      console.log(this.person.greet())
    }
  },
  created () {

  },
  mounted () {
    let vm = this
    state( vm.person, {
        Formal: {
            greet: function () { return "How do you do?"; }
        },
        Casual: {
            greet: function () { return "Hi!"; }
        }
    });
    
  }
}
</script>

<style lang="scss">
#app {

  body {
    font-family: 'Open Sans', sans-serif;

  }
  
  canvas {
    border:  1px dotted #ccc;
  }
  
  #text-explain {
    display: inline-block;
    font-size: 0.75em;
    margin-bottom: 1em;
  }

  .alert {
    color: tomato;
  }
}
</style>
