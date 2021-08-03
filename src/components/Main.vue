<template>
  <div class="container">
    <div class="jumbotron">
      <div class="mt-5 alert alert-success ">
        <h1 class="display-2">Hello, {{ name }}!</h1>
        <p class="lead">This is a simple hero unit, a simple jumbotron-style component for calling extra attention to featured content or information.</p>
        <hr class="my-4">
        <div class="card">
          <div class="card-body">
            <!--TODO: Burada space yada enter tusuna basildikca bir sonraki kelimeye gecmesini sagla.-->
            <!-- ornegin bir counter yap ve basildikca degerini arttir-->
            <!-- Bu kisimda wordsChecked dizisindeki degerlere gore kelimelerin arka planlarini ayarla-->
            <span class="words = ms-3 m-1" :class="key===counter ? 'onWord' : '' " v-for="(word, key) in wordsData" :key='key'>
              {{word}}
            </span>
          </div>
        </div>
      </div>
    </div>
    {{inputWord}}
    <div class="input-group input-group-lg">
      <!-- Kelimeler bittigi zaman kelime girmeyi engelle-->
      <input v-bind:disabled="isInputDisable" type="text" class="form-control" @keydown.space="nextWord" v-model="inputWord">
      <div class="input-group-append input-group-lg">
        <button @click='timer' class="btn btn-outline-success ms-2 "  type="button">
          <i class="far fa-clock"></i>
          {{ 9 >= second ? '0' + second : second }}
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
export default {
  name: "Main",
  props:{
    name : String,
  },
  data(){
    return {
      counter: 0,
      second: 0,
      isInputDisable: false,
      inputWord: '',
      wordsChecked: [],
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
    inputWord(value){
      value = value.trim();
      //console.log(value)
      //console.log(this.counter)
      this.wordsChecked[this.counter] = value === this.wordsData[this.counter];
    },
  },

  methods:{
    nextWord: function(){
      if(this.wordsData.length !==0) {
        this.counter += 1
        this.inputWord = ''
        //console.log(this.wordsChecked)

      }
      // Veri kalip kalmadigini kontrol ediyorum
      if(this.counter === this.wordsData.length) {
        this.isInputDisable = true
        alert('no words left')
      }
    },
    timer: function(){
      const vm = this;
      const _timer = setInterval(function () {
        if (vm.second >= 60) {
          clearInterval(_timer)
          alert('no time left')
        } else {
          vm.second += 1
          //console.log(vm.second)
        }
      }, 1000);
    }
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
</style>