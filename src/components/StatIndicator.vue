<template>
<div style="margin:5px;">
  {{ statName.toUpperCase() }}
  <div class="stat-bar row border">
    <div :class="'border text-center text-color ' + statName" :id="statName">{{ value }}</div>
  </div>
</div>
  
</template>

<script>
export default {
  props: { value: String(), statName: String() },
  methods:{
    calculateStats(){
      var element = document.getElementById(this.$props.statName)
      element.style.width = `${this.$props.value * 3}px`
    },
  },
  mounted(){
    this.calculateStats();
  },
  updated(){
    this.calculateStats();
  }
  
};
</script>

<style>
.text-center{
  text-align: center;
}
.text-color{
  color: white;
}
.stat-bar {
  width: 350px;
}
.stat-fill {
  height: 30px;
}
.hp {
  background-color: red;
}
.attack {
  background-color: orangered;
}
.defense {
  background-color: darkblue;
}
.special-attack {
  background-color: seagreen;
}
.special-defense {
  background-color: navy;
}
.speed {
  background-color: teal;
}
.border {
  border: 1px solid black;
  border-radius: 15px;
}
</style>