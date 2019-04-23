<template>
  <div class="myCanvas">
    <svg @click="changeColor" width="200" height="200">
        <circle cx="100" cy="100" r="30"></circle>
    </svg>
    <input type="text" id="text-input" v-model="color">
    <h1>Hello Canvas</h1>
    <div id="container"></div>    
    <!-- An explanation... --> 
    <div id="text-explain">...takes numbers between 1 and 10k</div>
    <input type="text" id="text-input" value="5000" v-model="insert">
    <span>{{ insert }}</span>
    <i class="i-home"></i>
  </div>
</template>

<script>
import * as d3 from "d3"

export default {
  name: 'myCanvas',
  data () {
      return {
          insert: 50,
          color: 'blue'
      }
  },
  props: {
    msg: String
  },
  mounted () {

      var data = [];
        d3.range(5000).forEach(function(el) {
        data.push({ value: el }); 
      });

      var width = window.innerWidth
        || document.documentElement.clientWidth
        || document.body.clientWidth;
      var height = window.innerHeight
        || document.documentElement.clientHeight
        || document.body.clientHeight;

      var canvas = d3.select('#container')
        .append('canvas')
        .attr('width', width/2)
        .attr('height', height/2);
      var context = canvas.node().getContext('2d');
    var customBase = document.createElement('custom');
        var custom = d3.select(customBase);
    //Build Element
    function databind (data) {
  // Bind data to custom elements.

        // Settings for a grid with 10 cells in a row, 
        // 100 cells in a block and 1000 cells in a row.
        var groupSpacing = 4; 
        var cellSpacing = 2; 
        var offsetTop = height / 5; 
        var cellSize = Math.floor((width - 11 * groupSpacing) / 100) - cellSpacing;

        var colourScale = d3.scaleSequential()
        colourScale.domain([0, 256]).interpolator(d3.interpolateRainbow)

        var join = custom.selectAll('custom.rect')
            .data(data);

        var enterSel = join.enter()
            .append('custom')
            .attr('class', 'rect')
            .attr("x", function(d, i) {
                var x0 = Math.floor(i / 100) % 10, x1 = Math.floor(i % 10);     
                return groupSpacing * x0 + (cellSpacing + cellSize) * (x1 + x0 * 10); })
            .attr("y", function(d, i) {
            var y0 = Math.floor(i / 1000), y1 = Math.floor(i % 100 / 10); 
            return groupSpacing * y0 + (cellSpacing + cellSize) * (y1 + y0 * 10); })
            .attr('width', 0)
            .attr('height', 0);

        join 
            .merge(enterSel)
            .transition()
            .attr('width', cellSize)
            .attr('height', cellSize)
            .attr('fillStyle', function(d) { return colourScale(d); });

        var exitSel = join.exit()
        .transition()
        .attr('width', 0)
        .attr('height', 0)
        .remove();
    }
    function draw() {
        // Draw the elements on the canvas.
            context.clearRect(0, 0, width, height); // Clear the canvas.

            var elements = custom.selectAll('custom.rect');
        // Grab all elements you bound data to in the databind() function.
        elements.each(function(d,i) { // For each virtual/custom element...
        var node = d3.select(this); 
        // This is each individual element in the loop. 
        
        context.fillStyle = node.attr('fillStyle'); 
        // Here you retrieve the colour from the individual in-memory node and set the fillStyle for the canvas paint
        context.fillRect(node.attr('x'), node.attr('y'), node.attr('width'), node.attr('height'));
        // Here you retrieve the position of the node and apply it to the fillRect context function which will fill and paint the square.
        });

    }
    
    // databind(data);
    // draw();

    databind(d3.range(333)); // Build the custom elements in memory.
    var t = d3.timer(function(elapsed) {
        
    draw();
    if (elapsed > 300) t.stop();
    }); // Timer running the draw function repeatedly for 300 ms.

  },
  methods: {
      databind (data) {

      },
      changeColor () {
          d3.select('circle').style('fill', this.color)
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.myCanvas {
 #container {
     border: solid 1px blue;
     
 }
 canvas {
    margin: auto 0;
    border: solid 1px green;
}
}
</style>
