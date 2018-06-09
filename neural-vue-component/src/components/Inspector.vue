<template>
  <div class="container-fluid">
    <div class="row">
      <div v-for="(p,index) in responseProfiles" class="container col-md-3" align=left>
        <span class="output-cell"><Chart ref='c' /></span>
        <span v-show='false' class="output-cell">{{doMath(index)}}</span>
      </div>
    </div>
  </div>
</template>

<script>

import Chart from '@/components/Chart';

export default {
  props: ['net'],
  components: {
    Chart
  },
  data() {
    return {
      responseProfiles: [
        [],[],[],[]
      ]
    }
  },
  mounted() {
  },
  methods: {
    doMath: function(index) {
      return index+1;
    },
    sampleResponseProfile: function() {

      this.responseProfiles= [
        [],[],[],[]
      ];

     if (this.net!==null) {
       var step = 0.1
       var min  = 0
       var max  = 1
       for(var xStep=min; xStep<=max; xStep+=step){
           for(var yStep=min; yStep<=max; yStep+=step){
               var out = this.net.activate([xStep, yStep])
               for(var o=0; o<out.length; o++){
                   this.responseProfiles[o].push({
                       x: xStep,
                       y: yStep,
                       activation: out[o]
                   })
               }
           }
       }
     }
     var comp = this;
     this.responseProfiles.map(function (o,idx) {
        comp.$refs.c[idx].draw(o);
     })
     return this.responseProfiles
   },
  }
}
</script>

<style scoped>

  .output-cell {
    width: 100%;
    display: inline-block;
  }
  .output-cell img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    cursor: pointer;
  }
</style>
