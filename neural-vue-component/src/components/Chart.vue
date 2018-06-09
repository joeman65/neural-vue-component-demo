<template>
  <canvas  class="canvas" ref="canvas" width='2000px' height='2000px' />
</template>

<script>

export default {
  methods: {
    draw: function (profile) {
      if (profile===undefined)
        return;
      var ctx = this.$refs.canvas.getContext("2d");
      try {
        var w = this.$refs.canvas.width;
        var h = this.$refs.canvas.height;
        ctx.clearRect(0,0,w,h);
        var comp = this;
        profile.map(function(o, index) {
          var x = ((w-200)*o.x)+100;
          var y = ((h-200)*o.y)+100;
          comp.drawNeuron(ctx,x,y,o.activation);
        })
      } catch (ex) {
      }
    },
    drawNeuron: function (ctx,x,y,v) {
      ctx.beginPath();
      ctx.fillStyle = this.getColor(v);
      ctx.strokeStyle = this.getColor(v);
      ctx.moveTo(x,y);
      ctx.arc(x,y,50,0,2*Math.PI);
      ctx.fill();
      ctx.stroke();
    },
    getColor: function (o) {
      return "rgb("+(255*o)+","+(255*o)+","+(155*o)+")"
    },
  }
}
</script>

<style scoped>

.canvas {
  background-color: #303030;
  left: 0px;
  top: 0px;
  width: 100%;
  height: auto;
  border: 1px solid #2f2f2f;
}
</style>
