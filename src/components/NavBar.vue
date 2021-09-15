<template>
  <section class="nav-section" id="title">
    <div class="container-fluid">
      <nav class="navbar navbar-expand-lg navbar-dark">
        <img
          src="https://www.pngrepo.com/download/4062/mars.png"
          alt="Mars svg image"
          width="45"
          height="39"
          class="d-inline-block align-text-top svg-img"
        />
        <a class="navbar-brand mb-0 h1">Mars Rover</a>
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNavAltMarkup"
          aria-controls="navbarNavAltMarkup"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div
          class="collapse navbar-collapse justify-content-end"
          id="navbarNavAltMarkup"
        >
          <ul class="navbar-nav">
            <li class="nav-item">
              <router-link to="/" class="nav-link">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/rovers" class="nav-link">Rovers</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/about" class="nav-link">About</router-link>
            </li>
          </ul>
        </div>
      </nav>
    </div>
  </section>
</template>

<script>
export default {
  name: "NavBar"
};
</script>

<style>
.container-fluid {
  padding: 3% 15% 7%;
}

.navbar {
  padding: 0 0 4.5rem;
}

.navbar-brand {
  font-family: "Ubuntu";
  font-size: 2rem;
  font-weight: 700;
}

.nav-item {
  padding: 8px 18px;
}

.nav-link {
  font-size: 1.2rem;
  font-family: "Montserrat";
  font-weight: 300;
}

.navbar-nav .nav-link {
  border-radius: 15px;
}

.navbar-nav .nav-link:hover {
  background-color: #c1440e;
}

.svg-img {
  margin-right: 7px;
}
</style>
