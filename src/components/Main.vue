<template>
  <div class="container">
    <div class="jumbotron">
      <div class="mt-5 alert alert-success ">
        <h1 class="display-2">Hello, {{ name }}!</h1>
        <p class="lead">This is a simple hero unit, a simple jumbotron-style component for calling extra attention to featured content or information.</p>
        <hr class="my-4">
        <h1 v-if="isInputDisable"
            class="display-6">
          GAME OVER
        </h1>
        <div class="card">
          <div class="card-body">
            <!--TODO: Burada space yada enter tusuna basildikca bir sonraki kelimeye gecmesini sagla.-->
            <!-- ornegin bir counter yap ve basildikca degerini arttir-->
            <!-- Bu kisimda wordsChecked dizisindeki degerlere gore kelimelerin arka planlarini ayarla-->

            <!--:class="checkLetter(key, word)"-->
            <span class="words ms-3 m-1 "
                  :class="checkLetter(key, word)"
                  v-for="(word, key) in wordsData"
                  :key='key'>
              <span :class="key===counter ? 'onWord' :  '' ">
                {{word}}
              </span>
            </span>
          </div>
        </div>
      </div>
    </div>
    {{inputWord}}
    <div class="input-group input-group-lg">
      <!--TODO: Kelimeler bittigi zaman kelime girmeyi engelle-->
      <!--TODO:Kelime girmeye basladigi an timer i baslat-->
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
        <button class="btn btn-outline-danger ms-2" type="button">
          Reset
          <i class="fas fa-undo-alt"></i>
        </button>
      </div>
    </div>
    {{wordsChecked}}
  </div>
</template>

<script>
//import axios from 'axios'
export default {
  name: "Main",
  props:{
    name : String,
  },
  'data'(){
    return {
      counter: -1,
      second: 0,
      trueCount: 0,
      isInputDisable: false,
      isTrueCss:'',
      inputWord: '',
      wordsChecked: [],
      //http://asdfast.beobit.net/docs/
      wordsData: ['Lorem',
        'ipsum',
        'dolor',
        'sit',
        'amet,',
        'consectetur',
        'adipisicing',
        'elit.',
        'Distinctio',
        'doloremque,',
        'qui!',
        'Aut,',
        'deleniti',
        'doloremque',
        'eligendi',
        'et',
        'iure',
        'magnam',
        'magni',
        'molestiae',
        'nobis',
        'officia',
        'quaerat',
        'reiciendis',
        'sunt,',
        'totam',
        'vitae!',
        'Alias,',
        'atque',
        'reprehenderit.',
      ],
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
        else{
          this.wordsChecked[this.counter] = value.trim() === this.wordsData[this.counter].trim();
          this.isTrueCss = 'bg-success'
        }
      }
      else {
        this.wordsChecked[this.counter] = false
        this.isTrueCss = 'bg-danger'
      }
    },
    /*isTrueCss: function(value){
      if (value !== '') {
        this.isTrueCss = 'bg-true'
      }
      else{
        this.isTrueCss= 'bg-false'
      }
    }*/
  },

  methods:{
    nextWord: function(){

      if(this.wordsData.length !==0) {
        console.log()
        this.inputWord.replace(' ', '') === this.wordsData[this.counter] ? this.trueCount ++ : null
        console.log(`trueCount ${this.trueCount}`)
        console.log(`falseCount ${this.wordsChecked.length - this.trueCount}`)
        this.counter += 1
        this.inputWord = ''
        //console.log(this.wordsChecked)
      }

      // Veri kalip kalmadigini kontrol ediyorum
      /**if(this.counter === this.wordsData.length) {
        this.isInputDisable = true
        alert('no words left')
        this.isInputDisable = true
      }*/
    },
    // Zaman kontrolunu yapiyorum burada
    timer: function(){
      if (this.counter === -1){
        this.counter = 0
        //this.$refs.customInput.focus()
        const vm = this;
        const _timer = setInterval(()=>{
          if (vm.second >= 5) {
            alert('no time left')
            /*
              sakin silme
            if (this.inputWord !== ' ') {
              console.log(this.wordsChecked.length)
            }
            else{
              console.log(this.wordsChecked.length-1)
            }*/
            clearInterval(_timer)
            this.isInputDisable = true
          } else {
            vm.second += 1
            //console.log(vm.second)
          }
        }, 1000);
      }
      /*else{
        console.log('emre')
      }*/

    },
    checkLetter : function (key, value){
     /* console.log(this.wordsData[key].length)
      console.log(value.length)
      console.log()*/
      if (this.wordsChecked[key] === true  && this.wordsData[key].length === value.length){//&& this.wordsData[key] === value
        return 'bg-true'
      }
      else if(this.wordsChecked[key] === false ){//&& this.wordsData[key] !== value
        return 'bg-false'
      }
    },
  },
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
</style>