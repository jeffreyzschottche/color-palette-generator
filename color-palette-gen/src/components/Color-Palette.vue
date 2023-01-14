<template>
    <h2 style='margin-top: -2em; margin-bottom: 0em;'> Color-Palette</h2>
    <div class="steps">

        <i >- Click a color to copy the hex code to clipboard</i>
        <i>- Hover it to lock a button so it stays when you generate new ones.</i>
        <i>- Press space to generate new colors</i>
 
    </div>
    <div class="colorSquareWrapper">
      <div class="colorSquare" v-for="color in colors" :key="color.hex" :style="{ backgroundColor: color.hex }" @click="onClick(color.hex)" @mouseover="onHover(color)" @mouseleave="onLeave">
          {{ color.name }} - {{ color.hex }}
          <button v-if="!lockedColors.includes(color) && hoveredColor === color" class="lockButton" @click="lockColor(color)">Lock</button>
          <button v-if="lockedColors.includes(color)" class="unlockButton" @click="unlockColor(color)">Unlock</button>
        </div>
    </div>
    <button @click="getColorCombination" class='generateButton'>Generate Colors</button>

    <div class="selected-colors">
        <h3>↓ All Selected Colors ↓</h3>
        <div class="selected-colors-small"> 
        <div v-for="color in lockedColors" :key="color.hex">
          <p>Name: {{ color.name }}</p>
          <p>Hex: {{ color.hex }}</p>
          <p>RGB: {{ color.rgb }}</p>
          <div class="color-preview" :style="{ backgroundColor: color.hex }">
            <b>Hex: {{ color.hex }}</b>
          </div>
        </div>
    </div>
      </div>
      <div id="snackbar"></div>

  </template>
  <script>
  import { colorCombinations } from '../colors'

export default {
name: 'ColorPalette',
data() {
return {
colors: colorCombinations.slice(0, 5),
hoveredColor: null,
lockedColors: [],
}
},
methods: {
getColorCombination() {
let newColors = colorCombinations
.filter(color => !this.lockedColors.includes(color))
.sort(() => Math.random() - 0.5)
.slice(0, 5 - this.lockedColors.length)
this.colors = [...this.lockedColors, ...newColors];
},

onClick(colorHex) {
navigator.clipboard.writeText(colorHex)
.then(() => {

// alert(colorHex + ' has been copied to clipboard');
        var x = document.getElementById("snackbar");
        x.innerHTML = colorHex + ' ' + 'copied to clickboard!';
        x.className = "show";
        setTimeout(function(){ x.className = x.className.replace("show", ""); }, 3000);

})
.catch(err => {
console.error('Failed to copy text: ', err);
});
},

onHover(color) {
this.hoveredColor = color;
},
onLeave() {
this.hoveredColor = null;
},
lockColor(color) {
this.lockedColors.push(color);
this.hoveredColor = null;
},
unlockColor(color) {
this.lockedColors = this.lockedColors.filter(c => c !== color);
}
},
mounted() {
    document.addEventListener("keyup", e => {
      if (e.code === "Space") {
        this.getColorCombination();
      }
    });
  },
beforeMount() {
this.getColorCombination()
}
}
</script>

  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap');
*{
    font-family: 'Bebas Neue', cursive;
}

  .colorSquare {
      width: 200px;
      height: 300px;
    /*  border: 2px solid black;
      margin-left: 1em;
      margin-right: 1em;
      */
      display: flex; 
      flex-direction: column;
      align-items: center;
      justify-content: center;
  }
  .colorSquareWrapper {
      display: flex;
      flex-direction: row;
      justify-content: center;   
      width: 100%; 
  }
  .generateButton {
      margin-top: 1em;
  }
  .selected-colors-small{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  .selected-colors-small div{
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .color-preview{
    width: 200px;
    height: 100px;
  }
.steps {
    display: flex;
    flex-direction: column;
    border: 2px solid rgb(40, 126, 255);
    justify-content: center;
    margin-left: auto;
    margin-right:auto;
    width: 40%;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    background: rgb(247, 247, 247);
}
button{
    border: 1px solid rgb(40, 126, 255);
    background-color: white;
    color: black;
    padding: 0.5em;
    transition: 0.5s ease-in;
}
button:hover{
    cursor: pointer;
    color: white;
    background-color: black;
    transition: 0.5s ease-in;
}



#snackbar {
  visibility: hidden;
  min-width: 250px;
  margin-left: -125px;
  background-color: #333;
  color: #fff;
  text-align: center;
  border-radius: 2px;
  padding: 16px;
  position: fixed;
  z-index: 1;
  left: 50%;
  bottom: 30px;
  font-size: 17px;
}

#snackbar.show {
  visibility: visible;
  -webkit-animation: fadein 0.5s, fadeout 0.5s 2.5s;
  animation: fadein 0.5s, fadeout 0.5s 2.5s;
}

@-webkit-keyframes fadein {
  from {bottom: 0; opacity: 0;} 
  to {bottom: 30px; opacity: 1;}
}

@keyframes fadein {
  from {bottom: 0; opacity: 0;}
  to {bottom: 30px; opacity: 1;}
}

@-webkit-keyframes fadeout {
  from {bottom: 30px; opacity: 1;} 
  to {bottom: 0; opacity: 0;}
}

@keyframes fadeout {
  from {bottom: 30px; opacity: 1;}
  to {bottom: 0; opacity: 0;}
}
  </style>