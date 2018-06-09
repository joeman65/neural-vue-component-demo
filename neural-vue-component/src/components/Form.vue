<template>
  <div>
      <h2>It is {{hour}}:00 in a {{dayType}}</h2>
      <form>
          <select id="day-input" v-model='dayType' name="dayType" @change='onChange1'>
              <option value="work day">Work day</option>
              <option value="weekend">Weekend</option>
          </select>
          <input id="hour-input" type="range" min="0" max="24" v-model='hour' name="hour" @change='onChange1' />
      </form>
      <br/>
  </div>
</template>

<script>

export default {
  props: ['currentInput','onChange'],
  data() {
    return {
      hour: 0 ,
      dayType: 'work day'
    }
  },
  mounted() {
    this.hour = this.currentInput.hour
    this.dayType = this.currentInput.dayType
  },
  methods: {
    onChange1: function(e){
      var name  = e.target.name
      var value = e.target.value

      var formValues = Object.assign({}, this.currentInput)
      formValues[name] = value

      // notify parent of form change
      this.onChange(formValues)
    }
  }
}
</script>

<style scoped>

#day-input {
    float: left;
    width: 20%;
    margin: 10px;
    margin-top: 0px;
}

#hour-input {
    float: left;
    width: 70%;
}

</style>
