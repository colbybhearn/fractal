<template>
  <div id="app">
    <div class='controls'>
    <label style='display:block'>Fractal</label>
    <select v-model="fractal" style='display:block;width:108px' v-on:change='update'>  
      <option>Triangle</option>
      <option>Snowflake</option>
    </select>
    <!-- <input step='20' type="radio" id='triangle' value="triangle" v-model="fractal"  v-on:change='update' style='width: 30px;display: inline;'><label for="triangle">Triangle</label><br>
    <input step='20' type="radio" id='snowflake' value="snowflake" v-model="fractal"  v-on:change='update'><label for="snowflake">Snowflake</label> -->
    <label>Size</label><input id='size' step='20' type="number" name="" v-model="size"  v-on:change='update'>
		<label>Generation</label><input id='gen' type="number" name="" v-model="gen" v-on:change='update'>    
    </div>
    <canvas id="canvas" height="1000" width="1400">		</canvas>
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
      fractal: 'Triangle',
      size : 550,
      gen: 3
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
      this.params.start.x = 50;
      this.params.start.y = 200;

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
      for(var i=0;i<points.length;i++) {

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
    factalTriangle.gen[1] = [{x:50,y:50,rot:Math.PI*(1/6),scale:500}];
    factalTriangle.fractalize = function(frac, gen){
      if(frac.gen[gen]===undefined)
        return [];

      var oldTris = frac.gen[gen];
      var newTris = [];
      // iterate points in this gen three at a time to make the new gen
      for(var i=0;i<oldTris.length;i+=1)
      {
        var p1 = oldTris[i];        
        var newTrisTemp = frac.nextGen(p1);
        newTrisTemp.forEach(function(p){
          newTris.push(p);
        })
      }
      return newTris;
    }
    factalTriangle.nextGen=function(t1){
      //take 2 points, return the 5 points
      // points are not x/y coordinates.
      // points are angle/length
      var tris = [];
      var trix = t1.x;
      var triy = t1.y;
      var scale = t1.scale;
      var rot = t1.rot;

// take this triangle, and build 4 from it
    // top
    // bottom left
    // bottom right
    // middle
      function getTriangle(x,y,scale,rot){
        return {
          x:x,
          y:y,
          scale:scale,
          rot:rot
        }
      }


      //turn one triangle into three (top, bl, br)
      // find up for this triangle.
      // use rot to calculate new center
      let t1HalfXDelta = .5*((Math.cos(rot)*scale));
      let t1HalfYDelta = .5*((Math.sin(rot)*scale));
      let t1Center = {
        x: trix + t1HalfXDelta,
        y: triy + t1HalfYDelta
      }
      tris[0] = getTriangle(t1Center.x, t1Center.y, scale*.5, rot) // same rotation

      let t2HalfXDelta = .5*((Math.cos(rot+(2*Math.PI/3) )*scale));
      let t2HalfYDelta = .5*((Math.sin(rot+(2*Math.PI/3) )*scale));
      let t2Center = {
        x: trix + t2HalfXDelta,
        y: triy + t2HalfYDelta
      }
      tris[1] = getTriangle(t2Center.x, t2Center.y, scale*.5, rot) // same rotation


      let t3HalfXDelta = .5*((Math.cos(rot+(4*Math.PI/3) )*scale));
      let t3HalfYDelta = .5*((Math.sin(rot+(4*Math.PI/3) )*scale));
      let t3Center = {
        x: trix + t3HalfXDelta,
        y: triy + t3HalfYDelta
      }
      tris[2] = getTriangle(t3Center.x, t3Center.y, scale*.5, rot) // same rotation

      
      let t4Center = {
        x: trix,
        y: triy
      }
      //tris[3] = getTriangle(t4Center.x, t4Center.y, scale*.5, rot+Math.PI) // same rotation




      //tris[1] = 
      //tris[2] = 

      return tris;
    }
    factalTriangle.drawGeneration=function(frac, gen){
      if(frac.gen[gen]===undefined)
        vc.getGen(frac, gen);
      var tris = frac.gen[gen];
      
      var base = 0;
      var rotation = 0;
      for(var i=0;i<tris.length;i++){
        
        let tri = tris[i];
      
        rotation = base+(0) + tri.rot;
        let p1 = {
          x: tri.x+(Math.cos(rotation)*tri.scale),
          y: tri.y+(Math.sin(rotation)*tri.scale)
        };
        rotation = base+(2*Math.PI/3) + tri.rot;
        let p2 = {
          x: tri.x+(Math.cos(rotation)*tri.scale),
          y: tri.y+(Math.sin(rotation)*tri.scale)
        };
        rotation = base+(4*Math.PI/3) + tri.rot;
        let p3 = {
          x: tri.x+(Math.cos(rotation)*tri.scale),
          y: tri.y+(Math.sin(rotation)*tri.scale)
        };

        vc.ctx.beginPath();
        vc.ctx.lineTo(p1.x,p1.y);
        vc.ctx.lineTo(p2.x,p2.y);
        vc.ctx.lineTo(p3.x,p3.y);
        vc.ctx.lineTo(p1.x,p1.y);

        var red = vc.getRandomInt(256);
        var blue = vc.getRandomInt(256);
        var green = vc.getRandomInt(256);

        vc.ctx.strokeStyle="#"+red.toString(16)+blue.toString(16)+green.toString(16);

        
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
