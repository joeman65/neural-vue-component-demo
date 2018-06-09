<template>
  <div class="container-fluid">
    <div class="row">
      <div class="container col-md-12" align=left>
        <div>
          <span id="logo">
            <a href="https://www.youtube.com/channel/UCD-HLhRV_4Z3sYGkgqAnIJw">
                <img src="./assets/images/pollo.png" width="100" />
            </a>
          </span>
          <span id="title">
              <h1>Neural Vue Component</h1>
              <h3>feat. Mr Pollo Watzlawick</h3>
          </span>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="container col-md-12" align=left>
        <Form :currentInput='state.currentInput' :onChange='changeCurrentInput' />
      </div>
    </div>
    <div class="row">
      <div class="container col-md-12" align=left>
        <Output :output='state.currentOutput' :correction='correctCurrentOutput' />
      </div>
    </div>
    <div class="row">
      <div class="container col-md-12" align=left>
        <inspect :net='brain' ref='inspector' />
      </div>
    </div>
  </div>
</template>

<script>

import { Architect } from 'synaptic'
import Form from '@/components/Form';
import Output from '@/components/Output';
import Inspector from '@/components/Inspector';

export default {
  name: 'NeuralVue',
  components: {
    Form,Output,inspect: Inspector
  },
  data() {
    return {
      brain: null,
      experience: [],
      state: {
        currentInput: {
          hour    : 4,
          dayType : "work day"
        },
        currentOutput: [0.5, 0.5, 0.5, 0.5]
      }
    }
  },
  mounted() {
    this.brain = new Architect.Perceptron(2, 5, 4)
  },
  methods: {
    inputPreparation: function(input){
      var scaledHour    = input["hour"] / 24.0
      var binaryDayType = input["dayType"] == "work day" ? 0.0 : 1.0
      return [ scaledHour, binaryDayType ]
    },
    changeCurrentInput: function(input){
      var inputVector = this.inputPreparation(input)
      var output = this.brain.activate(inputVector)
      this.state = {currentInput: input,currentOutput: output};
    },
    correctCurrentOutput: function(correctReply){
      var inputVector = this.inputPreparation(this.state.currentInput)
      this.experience.push({
        in  : inputVector,
        out : correctReply
      });

      // Train
      this.trainCycle()

      // Show new net output for current input
      var output = this.brain.activate(inputVector);
      this.state.currentOutput = output;
      this.$refs.inspector.sampleResponseProfile();
    },
    trainCycle: function(){
      var component = this

      var numExperiences = this.experience.length
      var learningRate   = 0.02// / numExperiences
      var epochs         = 50000

      for(var i=0; i<epochs; i++){
          this.experience.forEach(function(experience){
              component.brain.activate(experience.in)
              component.brain.propagate(learningRate, experience.out)
          })
      }
    },
  }
}
</script>

<style>
  html {
    background-color: #303030 !important;
  }

  div {
    color: #909090 ;
    background-color: #303030 ;
  }

  #logo {
      display: inline-block;
      margin-left: 30px;
  }
  #logo img {
      vertical-align: bottom;
  }

  #title {
      display: inline-block;
      padding: 30px;
  }
</style>
