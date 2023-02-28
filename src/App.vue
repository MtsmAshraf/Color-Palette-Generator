<template>
  <main  @keypress.space="getRandomColors()">
    <div class="overlay"></div>
    <button @click="clickThemeMethod('.theme'), changeBodyBg()" class="theme">
      <font-awesome-icon icon="fa-solid fa-code" />
    </button>    
    <div class="container">
      <h2>Try These Colors!</h2>
      <div class="cards">
        <div class="card">
          <div class="color-bg" @click="copyingMethod('#color-one span.copy')" id="color-one">
            <span :class=" rgb ? 'copy' : null" v-show="rgb">rgb(71,209,74)</span>
            <span :class=" hex ? 'copy' : null" v-if="hex">{{ hexSpanMethod(0,".color-bg span:first-child") }}</span>
            <font-awesome-icon icon="fa-solid fa-copy" /></div>
          <div class="color-code" @click="copyingMethod('#color-one span.copy')">Copy</div>
        </div>
        <div class="card">
          <div class="color-bg" @click="copyingMethod('#color-two span.copy')" id="color-two">
            <span :class=" rgb ? 'copy' : null"  v-show="rgb">rgb(71,209,74)</span>
            <span :class=" hex ? 'copy' : null" v-if="hex">{{ hexSpanMethod(1,".color-bg span:first-child") }}</span>
            <font-awesome-icon icon="fa-solid fa-copy" /></div>
          <div class="color-code" @click="copyingMethod('#color-two span.copy')">Copy</div>
        </div>
        <div class="card">
          <div class="color-bg" @click="copyingMethod('#color-three span.copy')" id="color-three">
            <span :class=" rgb ? 'copy' : null"  v-show="rgb">rgb(71,209,74)</span>
            <span :class=" hex ? 'copy' : null" v-if="hex">{{ hexSpanMethod(2,".color-bg span:first-child") }}</span>
            <font-awesome-icon icon="fa-solid fa-copy" /></div>
          <div class="color-code" @click="copyingMethod('#color-three span.copy')">Copy</div>
        </div>
        <div class="card">
          <div class="color-bg" @click="copyingMethod('#color-four span.copy')" id="color-four">
            <span :class=" rgb ? 'copy' : null"  v-show="rgb">rgb(71,209,74)</span>
            <span :class=" hex ? 'copy' : null" v-if="hex">{{ hexSpanMethod(3,".color-bg span:first-child") }}</span>
            <font-awesome-icon icon="fa-solid fa-copy" /></div>
          <div class="color-code" @click="copyingMethod('#color-four span.copy')">Copy</div>
        </div>
    </div>
    <p>Don't like them?</p>
    <button
     @click="
        clickMehtod('.again'),
        getRandomColors(),
        changeBgColor(true,'.cards','.user-color'),
        method(),
        clearInputs()" 
      @keypress.space="getRandomColors()"
      class="again">Generate Random Colors!</button>
    <span class="or">OR</span>
    <h4>Generate Matching Colors for your color</h4>
    <form action="">
      <button @click.prevent="rgb = false, hex = true, method()" v-if="rgb" class="converting-btn">Use HEX?</button>
      <input v-show="rgb" @input=" rgbInputMethod()" v-model="r" type="number" min="0" max="255" id="r" class="rgb" placeholder="R">
      <input v-show="rgb" @input=" rgbInputMethod()" v-model="g" type="number" min="0" max="255" id="g" class="rgb" placeholder="G">
      <input v-show="rgb" @input=" rgbInputMethod()" v-model="b" type="number" min="0" max="255" id="b" class="rgb" placeholder="B">
      <button @click.prevent="rgb = true, hex = false" v-show="hex" class="converting-btn">Use rgb?</button>
      <input v-show="hex"  @input="hexInputMethod()" v-model="hexacode" type="text" maxlength="9" minlength="4" id="hex" placeholder="HEX">
      <div class="user-color"></div>
      <button @click.prevent="clickMehtod('.generate-match'),getMatchedColors(),changeBgColor(false,'.cards','.user-color'),method()" class="generate-match">Generate</button>
    </form>
  </div>
  </main>
</template>

<script>
import { createApp, onUpdated } from 'vue'
import App from './App.vue'
import { library } from '@fortawesome/fontawesome-svg-core'
import { faMoon, faSun, faCopy, faCode } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
library.add(faMoon,faSun, faCopy , faCode)
createApp(App)
.component('font-awesome-icon', FontAwesomeIcon)
.mount('#app')
import axios from 'axios';
import { onMounted} from "vue";

export default {
  name: 'App',
  data(){
    return{
      nightMood: true,
      rgb:true,
      hex:false,
      r:null,
      g:null,
      b:null,
      hexacode: null
    }
  },
  components: {
    FontAwesomeIcon
  },
  setup(){
    onMounted(()=>{
      var url = "http://colormind.io/api/:splat200!";
      var data = {
        model : "default",
        input : [[Math.floor(256* Math.random()),Math.floor(256* Math.random()),Math.floor(256* Math.random())],"N","N","N","N"]
      }
      var http = new XMLHttpRequest();
      http.onreadystatechange = function() {
        if(http.readyState == 4 && http.status == 200) {
          var palette = JSON.parse(http.responseText).result;
          document.querySelectorAll(".card").forEach((card,index)=>{
              card.querySelector(".color-bg").style.cssText = `
                background-color: rgb(${palette[index][0]},${palette[index][1]},${palette[index][2]})
              `
              card.querySelector(".color-bg").querySelector("span").textContent = `rgb(${palette[index][0]},${palette[index][1]},${palette[index][2]})`
            })
        }
      }
      http.open("POST", url, true);
      http.send(JSON.stringify(data));
    })
  },
  methods:{
    clickMehtod(selector){
      document.querySelector(selector).classList.add("animated")
        document.querySelector("body").style.backgroundImage = "url(../src/assets/imgs/bg.jpg)"
      setTimeout(() => {
        document.querySelector(selector).classList.remove("animated")
      }, 1100);
    },
    clickThemeMethod(selector){
      // this.nightMood ? this.nightMood = false : this.nightMood = true
      window.open("https://moatasimashraf.000webhostapp.com/","_blank")
      this.clickMehtod(selector)
    },
    getRandomColors(){
      this.changeBodyBg();
      var url = "http://colormind.io/api/:splat200!";
      var data = {
        model : "default",
        input : [[Math.floor(256* Math.random()),Math.floor(256* Math.random()),Math.floor(256* Math.random())],"N","N","N","N"]
      }
      var http = new XMLHttpRequest();
      http.onreadystatechange = function() {
        if(http.readyState == 4 && http.status == 200) {
          var palette = JSON.parse(http.responseText).result;
          document.querySelectorAll(".card").forEach((card,index)=>{
              card.querySelector(".color-bg").style.cssText = `
                background-color: rgb(${palette[index][0]},${palette[index][1]},${palette[index][2]})
              `
              card.querySelector(".color-bg").querySelector("span").textContent = `rgb(${palette[index][0]},${palette[index][1]},${palette[index][2]})`
            })
        }
      }
      http.open("POST", url, true);
      http.send(JSON.stringify(data));
    },
    getMatchedColors(){
      this.changeBodyBg();
      if(this.r !== null && this.g !== null && this.b !== null){
        var url = "http://colormind.io/api/:splat200!";
        var data = {
          model : "default",
          input : [[this.r,this.g,this.b],"N","N","N","N"]
        }
        var http = new XMLHttpRequest();
        http.onreadystatechange = function() {
          if(http.readyState == 4 && http.status == 200) {
            var palette = JSON.parse(http.responseText).result;
            document.querySelectorAll(".card").forEach((card,index)=>{
                card.querySelector(".color-bg").style.cssText = `
                  background-color: rgb(${palette[index][0]},${palette[index][1]},${palette[index][2]})
                `
                card.querySelector(".color-bg").querySelector("span").textContent = `rgb(${palette[index][0]},${palette[index][1]},${palette[index][2]})`
              })
          }
        }
        http.open("POST", url, true);
        http.send(JSON.stringify(data));
      }else{
        console.log("error")
      }
    },
    convertToHex(decimal){
      var hexDigits = [];
      if(decimal === 0){
        hexDigits.unshift('00')
      }else{
        while(parseInt(decimal) > 0){
          var remainder = parseInt(decimal) % 16;
          if(remainder >= 10){
                  if(remainder === 10){
                    remainder = "A";
                  }else if(remainder === 11){
                    remainder = "B";
                  }else if(remainder === 12){
                    remainder = "C";
                  }else if(remainder === 13){
                    remainder = "D";
                  }else if(remainder === 14){
                    remainder = "E";
                  }else if(remainder === 15){
                    remainder = "F";
                  }
          }
          hexDigits.unshift(remainder)
          decimal = parseInt(parseInt(decimal) / 16);
        }      
      }
      var hexa = `${hexDigits.join("")}`
      return hexa.length > 1 ? `${hexDigits.join("")}` : `0${hexDigits.join("")}` ;
    },
    convertToHexFromRgb(r = 0,g = 0,b = 0){
      return `#${this.convertToHex(r) || '00'}${this.convertToHex(g) || '00'}${this.convertToHex(b) || '00'}`
    },
    changeBgColor(transparent,...selectors){
         if(!transparent){
          let red = parseInt(document.querySelectorAll("form input.rgb")[0].value);
          let green = parseInt(document.querySelectorAll("form input.rgb")[1].value);
          let blue = parseInt(document.querySelectorAll("form input.rgb")[2].value);
          selectors.forEach((selector) => {
          document.querySelector(selector).style.cssText = `
              background-color: rgb(${red}, ${green}, ${blue});
            `
          })
        }else{
          selectors.forEach((selector) => {
          document.querySelector(selector).style.cssText = `
              background-color: transparent;
            `
          })
        }
    },
    changeBodyBg(){
      if(this.nightMood){
        console.log("overlay bg changed")
        // document.querySelector(".overlay").style.backgroundImage = "url(http://localhost:8081/img/layered-peaks-haikei.7a0001d2.svg)"
      }else{
        console.log("overlay bg changed")
        // document.querySelector(".overlay").style.backgroundImage = "url(http://localhost:8081/img/layered-peaks-haikei-light.3f77d98c.svg)"
      }
    },
    copyingMethod(selector){
      navigator.clipboard.writeText(document.querySelector(selector).textContent);
      alert("Copied the text: " + document.querySelector(selector).textContent);
    },
    convertToDecimal(hex){
      hex = hex.split("")
      let hexBase = [];
          for(let i = 0; i < hex.length; i++){
            hexBase.unshift(16 ** i)
          }
          let decimal = 0
          hexBase.forEach((ele, index) => {
            if(hex[index] === "A" || hex[index] === "a"){
              hex[index] = 10
            }else if(hex[index] === "B" || hex[index] === "b"){
              hex[index] = 11
            }else if(hex[index] === "C" || hex[index] === "c"){
              hex[index] = 12
            }else if(hex[index] === "D" || hex[index] === "d"){
              hex[index] = 13
            }else if(hex[index] === "E" || hex[index] === "e"){
              hex[index] = 14
            }else if(hex[index] === "F" || hex[index] === "f"){
              hex[index] = 15
            }
            decimal += ele * hex[index]
          })
          console.log(decimal)
          return decimal
    },
    hexInputMethod(){
      let v = document.querySelector("#hex").value;
      console.log(v.slice(1,3).split(""))
      console.log(this.convertToDecimal(v.slice(1,3)))
      this.r = this.convertToDecimal(v.slice(1,3))
      console.log(this.r)
      this.g = this.convertToDecimal(v.slice(3,5))
      this.b = this.convertToDecimal(v.slice(5,7))
    },
    rgbInputMethod(){
      if((this.r !== null && this.g !== null && this.b !== null))
        this.hexacode = this.convertToHexFromRgb(this.r,this.g,this.b);
    },
    hexSpanMethod(i, selector){
      // let test = "rgb(222,222,255)"
      return this.convertToHexFromRgb(parseInt(document.querySelectorAll(selector)[i].textContent.split("(")[1].split(",")[0]),parseInt(document.querySelectorAll(selector)[i].textContent.split("(")[1].split(",")[1]),parseInt(document.querySelectorAll(selector)[i].textContent.split("(")[1].split(",")[2]))
    },
    clearInputs(){
      document.querySelectorAll("input").forEach((input) => {
        input.value = "";
        console.log("cleared")
      })
    },
    method(){
      window.onload = () => {
        document.querySelectorAll(".color-bg span:first-child").forEach((span,index) => {
        document.querySelectorAll(".color-bg span:last-child")[index].textContent = this.convertToHexFromRgb(parseInt(span.textContent.split("(")[1].split(",")[0]),parseInt(span.textContent.split("(")[1].split(",")[1]),parseInt(span.textContent.split("(")[1].split(",")[2]))
      })
      }
    }
  },
  // computed:{
  //   hexacode(){
  //     return this.convertToHexFromRgb(this.r,this.g,this.b)
  //   } ,
  // },
  watch:{
    nightMood:{
      handler(v){
        if(v){
          document.documentElement.style.setProperty('--main', '#001427');
          document.documentElement.style.setProperty('--sec', '#fff');
        }else{
          
          document.documentElement.style.setProperty('--main', '#fff');
          document.documentElement.style.setProperty('--sec', '#001427');
        }
      },
      immediate:true,
    },
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nova+Round&display=swap');
*{
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  transition: var(--main-transition);
}
:root{
  --main: #fff;
  --sec: #1f2b38;
  --main-transition: all 0.2s 0s linear
}
body{
  /* background-color: var(--main); */
  /* background-image: url(../src/assets/imgs/bg.jpg); */
  background-size: cover;
  position: relative;
  font-family: "Nova Round", sans-serif;
}
.overlay{
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  background-color: black;
  opacity: 1;
  width: 100vw;
  height: 100vh;
  background-image: url(./assets/imgs/layered-peaks-haikei.svg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #001427;
  position: relative;
}
main *:not(.overlay){
  position: relative;
}
button,input{
  font-family: "Nova Round", sans-serif;
}
button.theme{
  padding: 6px 2px;
  border: none;
  background-color: var(--main);
  color: var(--sec);
  cursor: pointer;
  position: fixed;
  left: 20px;
  top: 20px;
  display: flex;
  justify-content: center;
  box-shadow: 0px 2px 0px 2px var(--sec);
  align-items: center;
  font-size: 20px;
  border-radius: 10px;
  width: 40px;
  z-index: 4;
}
button.theme.animated{
  animation: click-theme-effect 0.4s linear 0s 1 forwards;
}
button.theme:hover{
  box-shadow: 0px 2px 0px 2px yellow;
}
button.theme svg.fa-code{
  color: yellow;
}
.container{
  padding-right: 20px;
  padding-left: 20px;
  padding-top: 20px;
  margin-right: auto;
  margin-left: auto;
}
.container > h2{
  color: var(--sec);
}
.container .cards{
  display: grid;
  grid-template-columns: repeat(4,1fr);
  padding-top: 20px;
  padding-bottom: 30px;
  justify-content: space-around;
  /* gap: 10px; */
  row-gap: 30px;
  margin-top: 10px;
  border-radius: 6px;
  border: 3px solid #f5730a;
  animation: cards-parent-animation 0.5s ease-in-out 0.2s 1 normal forwards;
  opacity: 0;
}
.container .cards .card{
  opacity: 0;
  position: relative;
  background-color: var(--main);
  border: 1px solid #001427;
  height: 250px;
  cursor: pointer;
  animation: card-animation 0.5s ease-in-out 0.7s 1 normal forwards;
}
.container .cards .card:hover .color-bg{
  transform: scale(1.01);
}
.container .cards .card:nth-of-type(2){
  animation: card-animation 0.5s ease-in-out 0.8s 1 normal forwards;
}
.container .cards .card:nth-of-type(3){
  animation: card-animation 0.5s ease-in-out 0.9s 1 normal forwards;
}
.container .cards .card:nth-of-type(4){
  animation: card-animation 0.5s ease-in-out 1s 1 normal forwards;
}
.container .cards .card .color-bg{
  background-color: salmon;
  height: 100%;
  display: flex;
  flex-direction: column;
  transition-duration: 0s;
  justify-content: center;
  box-shadow: 0px 2px 0px 2px #ddd;
  align-items: center;
  transition: var(--main-transition);
}
.container .cards .card .color-bg span{
  padding: 5px;
  border-top: 1px solid #fff;
  border-bottom: 1px solid #fff;
  color: #fff;
  transition-timing-function: ease-in;
  background-color: #00142744;
  opacity: 1;
  transform: translateY(30px);
}
.container .cards .card .color-bg svg{
  padding: 10px;
  border-top: 1px solid #fff;
  border-bottom: 1px solid #fff;
  color: var(--sec);
  transition-timing-function: ease-in;
  background-color: var(--main);
  font-size: 18px;
  border-radius: 50%;
  opacity: 0;
  transform: translateY(30px);
}
.container .cards .card .color-bg:hover{
  box-shadow: 0px 2px 0px 2px #f5730a;
}
.container .cards .card .color-bg:hover span{
  opacity: 0;
  transform: translateY(-30px);
}
.container .cards .card .color-bg:hover svg{
  opacity: 1;
  transform: translateY(0px);
}
.container .cards .card .color-code{
  position: absolute;
  left: 50%;
  bottom: 0px;
  transform: translate(-50%,50%);
  background-color: var(--main);
  padding-top: 5px;
  box-shadow: 1px 2px 0px 2px #888;
  padding-bottom: 5px;
  border-radius: 20px;
  width: 70px;
  text-align: center;
  color:var(--sec);
  display: none;
}
.container > p{
  color: var(--sec);
  margin-top: 20px;
}
.container > .or{
  position: relative;
  color: var(--sec);
  padding-left: 10px;
  padding-right: 10px;
}
.container  > .or::before{
  position: absolute;
  content:'';
  left: 0;
  top: 50%;
  transform: translate(-100%,-50%);
  background-color: #f5730a;
  width: 100px;
  height: 3px;
}
.container  > .or::after{
  position: absolute;
  content:'';
  right: 0;
  top: 50%;
  transform: translate(100%,-50%);
  background-color: #f5730a;
  width: 100px;
  height: 3px;
}
.container > button:not(.theme){
  height: 30px;
  padding-right: 10px;
  padding-left: 10px;
  border: none;
  background-color: var(--main);
  color: #f5730a;
  cursor: pointer;
  display: flex;
  justify-content: center;
  box-shadow: 0px 3px 0px 2px #000001;
  border-bottom: 1px solid rgb(134, 237, 134);
  align-items: center;
  font-size: 20px;
  border-radius: 10px;
  margin: 10px auto 20px;
  font-weight: bold;
}
button:not(.theme).animated{
  animation: click-effect 0.4s linear 0s 1 forwards;
}
.container > h4{
  color: var(--sec);
  text-transform:lowercase;
  margin-bottom: 10px;
}
.container > h4::first-letter{
  text-transform: uppercase;
}
.container form > *:not(label){
  padding: 3px 10px;
  border: none;
  background-color: var(--main);
  color: var(--sec);
  cursor: pointer;
  display: flex;
  justify-content: center;
  box-shadow: 0px 3px 0px 2px var(--sec);
  align-items: center;
  font-size: 20px;
  border-radius: 10px;
  border-top: 2px solid transparent;
  margin: 10px auto 20px;
}
.container form > input{
  display: inline;
  text-align: center;
  margin-left: 5px;
  margin-right: 5px;
  appearance: tex;
}
.container form > input:not(#hex){
  width: 60px;
}
.container form > input#hex{
  width: 150px;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
/* input[type=number] { */
  /* -moz-appearance: textfield; */
/* } */
.container form > input:focus{
  outline: none;
  border-top: 2px solid var(--sec);
}
.container form .user-color{
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background-color: transparent;
}
.container form button{
  background-color: #f5730a;
  color: var(--main);
  box-shadow: 0px 3px 0px 2px #bc5806 ;
  font-weight: bold;
  border-bottom: 2px solid rgb(134, 237, 134);
}
.container form button.converting-btn{
  font-size: 14px;
  background-color: #888;
  box-shadow: 0px 3px 0px 2px #5e5d5d ;
  border-bottom: 2px solid rgb(189, 193, 189);
}
.container form button:hover{
  box-shadow: 0px 3px 0px 2px var(--sec) ;
}
@media (min-width:767px) {
  .container{
    width: 750px;
  }
}
@media (min-width:992px) {
  .container{
    width: 980px;
  }
}
@media (min-width:1200px) {
  .container{
    width: 1190px;
  }
}
@media (max-width:992px) {
  .container .cards{
    grid-template-columns: repeat(3,1fr);
    row-gap: 2px;
  }
  .container .cards .color-code {
    /* transform: translate(-50%,-0%) !important; */
    z-index: 2;
  }
}
@media (max-width:767px) {
  .container .cards{
    grid-template-columns: repeat(2,1fr);
  }
  
}
@keyframes click-theme-effect {
  0%{
    box-shadow: 0px 0px 0px 0px yellow;
    transform: translate(2px,3px);
  }
  100%{
   box-shadow: 0px 2px 0px 2px yellow;
   transform: translate(0px,0px);
  }
}
@keyframes click-effect {
  0%{
    box-shadow: 0px 0px 0px 0px var(--sec);
    transform: translate(0px,3px);
  }
  100%{
   box-shadow: 0px 3px 0px 2px var(--sec);
   transform: translate(0px,0px);
  }
}
@keyframes cards-parent-animation {
  0%{
    transform: translateY(200px);
    opacity: 0;
  }
  100%{
    transform: translateY(0px);
    opacity: 1;
  }
}
@keyframes card-animation {
  0%{
    transform: translateX(100px);
    opacity: 0;
  }
  100%{
    transform: translateX(0px);
    opacity: 1;
  }
}
</style>
