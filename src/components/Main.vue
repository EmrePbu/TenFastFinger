<template>
  <div class="container">
    <div class="jumbotron">
      <div class="mt-5 alert alert-success ">
        <h1 class="display-2">Hello, {{ name }}!</h1>
        <p class="lead">This is a simple hero unit, a simple jumbotron-style component for calling extra attention to featured content or information.</p>
        <hr class="my-4">
        <div class="card">
          <div class="card-body">
            <span v-for="(word, key) in wordsData" :key='key'>
              {{word.trim()+' '}}
            </span>
          </div>
        </div>
      </div>
    </div>
    <div class="input-group input-group-lg" >
      <input type="text" class="form-control">
      <div class="input-group-append input-group-lg">
        <button class="btn btn-outline-success ms-2" type="button">
          <i class="far fa-clock"></i>
          1:30
        </button>
        <button class="btn btn-outline-danger ms-2" type="button">
          Reset
          <i class="fas fa-undo-alt"></i>
        </button>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "Main",
  data(){
    return {
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
        'reprehenderit.',],
    }
  }
}
</script>

<style scoped>

</style>