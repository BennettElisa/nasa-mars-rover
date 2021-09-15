<template>
  <div class="container">
    <h1>NASA Mars Rovers</h1>
    <div class="row">
      <div class="col-lg-4">
        <div class="card">
          <img
            class="card-img-top"
            src="../assets/spirit-rover.jpeg"
            alt="Spirit Rover"
          />
        </div>
        <div class="card-body">
          <h3>Spirit Rover</h3>
          <h5>2004-2010</h5>
          <p>Spirit Rover is a twin to Opportunity Rover.</p>
        </div>
      </div>
      <div class="col-lg-4">
        <div class="card">
          <img
            class="card-img-top"
            src="../assets/curiosity-rover.jpeg"
            alt="Curiosity Rover"
          />
        </div>
        <div class="card-body">
          <h3>Curiosity Rover</h3>
          <h5>2012-Current</h5>
          <p>
            Curiosity began driving in August of 2012 and is still going strong.
          </p>
        </div>
      </div>
      <div class="col-lg-4">
        <div class="card">
          <img
            class="card-img-top"
            src="../assets/opportunity-rover.jpeg"
            alt="Opportunity Rover"
          />
        </div>
        <div class="card-body">
          <h3>Opportunity Rover</h3>
          <h5>2004-2018</h5>
          <p>Opportunity Rover lost contact with Earth in 2018.</p>
        </div>
      </div>
    </div>
    <router-link to="/">Go Back</router-link>
  </div>
</template>

<script>
export default {
  name: "Rovers"
};
</script>

<style scoped>
img {
  width: 100%;
}

@media (max-width: 1028px) {
  .container {
    display: flex;
    flex-wrap: wrap;
  }
}
</style>
