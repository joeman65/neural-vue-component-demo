<template>
  <div class="container-fluid">
    <div class="row">
      <div  v-for="(image,index) in images" class="container col-md-3" align=left>
        <span class="output-cell">
          <img :src='getImg(index)' :width='getWidth(index)' v-on:click="prepareCorrection(index)">
        </span>
        <span v-show='false' class="output-cell">{{doMath(index)}}</span>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  props: ['output','correction'],
  data() {
    return {
      images: ["bed","working","pizza","party"]
    }
  },
  methods: {
    doMath: function(index) {
      return index+1;
    },
    prepareCorrection: function(index) {
      var correctOutput = this.output.map(function(o){
        return 0.0
      })
      correctOutput[index] = 1.0

      this.correction(correctOutput)
    },
    getImg: function(index) {
      try {
        return require('../assets/images/'+this.images[index]+'.png');
      }
      catch (ex) {
          console.log(ex);
      }
    },
    getWidth: function(index) {
      return 50 + ( 100 * this.output[index] );
    }
  }
}
</script>

<style scoped>

  .output-cell {
    width: 25%;
    display: inline-block;
  }
  .output-cell img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    cursor: pointer;
  }
</style>
