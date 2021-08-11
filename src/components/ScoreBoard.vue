<template>
  <div class="jumbotron m-5">
    <div class="card alert bg-accuracy d-flex justify-content-center">
      <!--<img class="card-img-top" src="https://via.placeholder.com/150/24f355" alt="Card image cap">-->
      <div class="card-body">
        <h1 class="card-title d-flex justify-content-center display-3">{{trueValue+falseValue}} WPM</h1>
        <p class="lead">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item d-flex justify-content-between bg-accuracy mt-2">
          <span class="display-6">Accuracy</span>
          <span class="display-6">%{{accuracy}}</span>
        </li>
        <li class="list-group-item d-flex justify-content-between bg-true mt-2">
          <span class="display-6">Correct Word </span>
          <span class="display-6">{{trueValue}}</span>
        </li>
        <li class="list-group-item d-flex justify-content-between bg-false mt-2">
          <span class="display-6">Wrong Word</span>
          <span class="display-6">{{falseValue}}</span>
        </li>
      </ul>
      <div class="card-body">
        <button class="card-link btn btn-outline-danger ms-2"><i class="fa fa-share"></i> Share</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ScoreBoard",
  props:{
    accuracy: Number,
    trueValue: Number,
    falseValue: Number,
  }
}
</script>

<style scoped>
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