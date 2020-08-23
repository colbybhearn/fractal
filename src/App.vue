<template>
  <div id="app">
    <div class='controls'>
    <label style='display:block'>Fractal</label>
    <select v-model="fractal" style='display:block;width:108px'>  
      <option>Triangle</option>
      <option>Snowflake</option>
    </select>
    <!-- <input step='20' type="radio" id='triangle' value="triangle" v-model="fractal"  v-on:change='update' style='width: 30px;display: inline;'><label for="triangle">Triangle</label><br>
    <input step='20' type="radio" id='snowflake' value="snowflake" v-model="fractal"  v-on:change='update'><label for="snowflake">Snowflake</label> -->
    <label>Size</label><input id='size' step='20' type="number" name="" v-model="size"  v-on:change='update'>
		<label>Generation</label><input id='gen' type="number" name="" v-model="gen" v-on:change='update'>    
    </div>
    <canvas id="canvas" height="1000" width="1000">		</canvas>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data: function() {
    return {
      params: [],
      generations: [],
      generation: 0,
      ctx: {},
      fractal: '',
      size : 560,
      gen: 1
    }
    },
    mounted: function(){
      this.ctx = document.getElementById('canvas').getContext('2d')
      this.update();
    },
  methods:{    
    update: function(){
      this.draw();
    },
    draw: function()
    {
      let vc = this;
      var convas = document.getElementById('canvas');
      if(typeof this.ctx === 'undefined')
        return;
      this.ctx.clearRect(0, 0, canvas.width, canvas.height);
      this.params.start = {};
      this.params.start.x = 100;
      this.params.start.y = 100;

      let fractalSnowFlake = {};
      fractalSnowFlake.gen = [];
      //var size = document.getElementById('size').value;
      fractalSnowFlake.gen[1] = [{x:120,y:vc.size},{x:120,y:vc.size},{x:120,y:vc.size}];
      fractalSnowFlake.fractalize = function(frac, gen){
      if(frac.gen[gen]===undefined)
        return [];

      var oldPoints = frac.gen[gen];
      var newPoints = [];
      // iterate points in this gen one at a time to make the new gen
      for(var i=0;i<oldPoints.length;i++)
      {
        var p1 = oldPoints[i];			
        var newPointsTemp = frac.nextGen(p1);

        newPointsTemp.forEach(function(p){
          newPoints.push(p);
        })
        
      }
      return newPoints;
    }
    fractalSnowFlake.nextGen=function(p1){
      //take 2 points, return the 5 points
      var points = [];
      var length1 = p1.y;
      var angle = p1.x;
      var qlen = length1/3.0;
      points[0] = {x:angle,y:qlen}; // 120
      points[1] = {x:-60,y:qlen};	// -60
      points[2] = {x:120,y:qlen}; // 120
      points[3] = {x:-60,y:qlen}; // -60
      return points;
    }
    fractalSnowFlake.drawGeneration=function(frac, gen){
      if(frac.gen[gen]===undefined)
        vc.getGen(frac, gen);
      var points = frac.gen[gen];
      
      var rotation = 0;
      for(var i=0;i<points.length;i++){

        vc.ctx.beginPath();
        vc.ctx.lineTo(0,0);
        rotation=0;
        rotation += (-points[i].x*Math.PI/180);
        vc.ctx.rotate(rotation);
        vc.ctx.lineTo(points[i].y,0);

        var red = vc.getRandomInt(256);
        var blue = vc.getRandomInt(256);
        var green = vc.getRandomInt(256);

        vc.ctx.strokeStyle="#"+red.toString(16)+blue.toString(16)+green.toString(16);

        vc.ctx.translate(points[i].y,0);
        vc.ctx.stroke();
      }
    }


    let factalTriangle = {};
    factalTriangle.gen = [];
    //var size = document.getElementById('size').value;
    factalTriangle.gen[1] = [{x:120,y:vc.size},{x:120,y:vc.size},{x:120,y:vc.size}];
    factalTriangle.fractalize = function(frac, gen){
      if(frac.gen[gen]===undefined)
        return [];

      var oldPoints = frac.gen[gen];
      var newPoints = [];
      // iterate points in this gen three at a time to make the new gen
      for(var i=0;i<oldPoints.length;i+=3)
      {
        var p1 = oldPoints[i];
        var p2 = oldPoints[i+1];
        var p3 = oldPoints[i+2];			
        var newPointsTemp = frac.nextGen(p1,p2,p3);

        newPointsTemp.forEach(function(p){
          newPoints.push(p);
        })
      }
      return newPoints;
    }
    factalTriangle.nextGen=function(p1,p2,p3){
      //take 2 points, return the 5 points
      var points = [];
      var length1 = p1.y;
      var angle = p1.x;
      var qlen = length1/2.0;
      //turn one triangle into three (top, bl, br)
      points[0] = {x:p1.x+120,y:qlen}; //
      points[1] = {x:120,y:qlen};	//
      points[2] = {x:120,y:qlen}; // 

      points[3] = {x:120,y:qlen}; // position
      
      points[4] = {x:0,y:qlen};	//
      points[5] = {x:120,y:qlen}; // 
      points[6] = {x:120,y:qlen}; //

      points[7] = {x:240,y:qlen};	// position

      points[8] = {x:240,y:qlen}; //
      points[9] = {x:120,y:qlen}; //
      points[10] = {x:120,y:qlen};	//

      points[11] = {x:90,y:qlen};	// dummy

      return points;
    }
    factalTriangle.drawGeneration=function(frac, gen){
      if(frac.gen[gen]===undefined)
        vc.getGen(frac, gen);
      var points = frac.gen[gen];
      
      var rotation = 0;
      for(var i=0;i<points.length;i++){

        vc.ctx.beginPath();
        vc.ctx.lineTo(0,0);
        rotation=0;
        rotation += (-points[i].x*Math.PI/180);
        vc.ctx.rotate(rotation);
        vc.ctx.lineTo(points[i].y,0);

        var red = vc.getRandomInt(256);
        var blue = vc.getRandomInt(256);
        var green = vc.getRandomInt(256);

        vc.ctx.strokeStyle="#"+red.toString(16)+blue.toString(16)+green.toString(16);

        vc.ctx.translate(points[i].y,0);
        vc.ctx.stroke();
      }
    }

    let center = {};
    center.x = vc.size*1.0;
    center.y = vc.size*1.0;

    vc.ctx.translate(center.x,center.y);
    var g= vc.gen;
    for(g=g;g>0;g--)
      if(vc.fractal==='Triangle')
        factalTriangle.drawGeneration(factalTriangle, g);
      else
        fractalSnowFlake.drawGeneration(fractalSnowFlake,g);
    
    vc.ctx.translate(-center.x,-center.y);
    },

    getGen: function(frac, gen){
	if(frac.gen[gen-1] === undefined)
		this.getGen(frac, gen-1);
	frac.gen[gen] = frac.fractalize(frac, gen-1);
},
getRandomInt:function(max) {
  return Math.floor(Math.random() * Math.floor(max));
}
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.controls{
  position: absolute;
  background: gray;
}

.controls input{
  display:block;
  width: 100px;
  text-align: center;
}
</style>
