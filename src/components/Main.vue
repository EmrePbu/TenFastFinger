<template>
  <div class="container">
    <div class="jumbotron" v-if="!isInputDisable">
      <div class="mt-5 alert alert-success">
        <h1 class="display-2">and You the fastest shifter alive</h1>
        <p class="lead"> Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusantium, atque consequatur dolor dolore dolorum esse id iste iure </p>
        <hr class="my-4">
        <div class="card">
          <div class="card-body">
            <!--:class="checkLetter(key, word)"-->
            <span class="words ms-3 m-1 "
                  :class="checkLetter(key, word)"
                  v-for="(word, key) in wordsData.filter((data,index) => index<20)"
                  :key='key'>
              <span :class="key===counter%20 ? 'onWord' :  '' ">
                {{word}}
              </span>
            </span>
          </div>
        </div>
      </div>
    </div>
    <ScoreBoard v-if="isInputDisable" :accuracy='accuracy' :trueValue='trueCount' :falseValue='falseCount'></ScoreBoard>
    <div class="input-group input-group-lg"
         v-if="!isInputDisable">
      <input type="text"
             class="form-control"
             :disabled="isInputDisable"
             @keydown.space="nextWord"
             v-model="inputWord">
      <div class="input-group-append input-group-lg">
        <button @click='timer'
                class="btn btn-outline-success ms-2"
                type="button"
                disabled>
          <i class="far fa-clock"></i>

          {{ 9 >= second ? '0' + second : second }} s
        </button>
        <!--TODO: Timer resetle-->
        <button class="btn btn-outline-danger ms-2" type="button" @click='getWords'>
          Reset
          <i class="fas fa-undo-alt"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
//import axios from 'axios'
import ScoreBoard from "@/components/ScoreBoard";

export default {
  name: "Main",
  components:{
    ScoreBoard,
  },
  'data'(){
    return {
      counter: -1,
      second: 60,
      trueCount: 0,
      falseCount: 0,
      accuracy: 0,
      isInputDisable: false,
      inputWord: '',
      wordsChecked: [],
      wordsData:[],
    }
  },
  watch:{
    inputWord: function (value) {
      value = value.trim();
      //console.log(value)
      //console.log(this.counter)
      // Girilem kelimenin dogrulugunu kontrol ediyorum.
      /* if (this.wordsData[this.counter] ===true){
        return 'bg-true'
      }
      else if(this.wordsData[this.counter]===false){
        return 'bg-false'
      }*/
      if (value !== '') {
        //.slice(0, value.length)
        //this.wordsChecked[this.counter] = value.replace(' ', '') === this.wordsData[this.counter].replace(' ', '');
        if (this.counter === -1){
          this.timer()
        }
        else if(this.counter !==-1){

          //console.log(value.trim() === this.wordsData[this.counter].trim())
          this.wordsChecked[this.counter] = value.trim() === this.wordsData[this.counter].trim();
        }
      }
    },
  },

  methods:{
    getWords: function(){
      fetch('https://random-word-api.herokuapp.com/word?number=300')
          .then(response => response.json())
          .then(data => this.wordsData = data);
    },
    nextWord: function(){
      if(this.wordsData.length !==0) {
        //TODO: Hata var kontrol et ilk basta bosluk girilirse
        this.inputWord.replace(' ', '') === this.wordsData[this.counter].replace(' ', '') ? this.trueCount ++ : this.falseCount++
        this.counter += 1
        this.inputWord = ''
        if (this.counter >= 20){
          // TODO: Burada kelimeleri degistirirken cssleride resetle
          this.wordsData.splice(0,20)
        }
      }

      // Veri kalip kalmadigini kontrol ediyorum
      /*
      if(this.counter === this.wordsData.length) {
        this.isInputDisable = true
        alert('no words left')
        this.isInputDisable = true
      }
      */
    },
    // Zaman kontrolunu yapiyorum burada
    timer: function(){
      if (this.counter === -1){
        this.counter = 0
        //this.$refs.customInput.focus()
        const vm = this;
        const _timer = setInterval(()=>{
          if (vm.second <= 0) {
            //alert('no time left')
            /*
            *     100           this.wordsData.length-this.falseCount
            *
            *      ?            this.trueCount
            * */
            /*
            * We take the number and add a very small number, Number.EPSILON, to ensure the number’s accurate rounding.
            * We then multiply by number with 100 before rounding to extract only the two digits after the decimal place.
            * Finally, we divide the number by 100 to get a max of 2 decimal places.
            * */
            this.accuracy = Math.round((this.trueCount * 100 / (this.trueCount+this.falseCount) + Number.EPSILON) * 100) / 100

            clearInterval(_timer)
            this.isInputDisable = true
          } else {
            vm.second -= 1
          }
        }, 1000);
      }
    },
    //TODO: 20 kelime sonra css leri resetle
    checkLetter : function (key, value){
      if (this.wordsChecked[key] === true  && this.wordsData[key].length === value.length){
        return 'bg-true'
      }
      else if(this.wordsChecked[key] === false ){
        return 'bg-false'
      }
    },
  },
  beforeMount() {
    this.getWords()
  }
}
</script>

<style scoped>
.words{
  font-size: 1.5rem;
  display: inline-block;
}
.onWord{
  background-color:rgba(150,150,150,0.3);
  padding:2px 5px;
  border-radius: 3px;
}
.bg-true{
  background-color: rgba(150,250,150,0.3);
  border-radius: 3px;
}
.bg-false{
  background-color: rgba(250,150,150,0.3);
  border-radius: 3px;
}
.bg-accuracy{
  background-color: rgba(150,150,250,0.3);
  border-radius: 3px;
}
</style>