<template>
  <router-link to="/"></router-link>
  <div>
    <h1 id="title">Welcome to Mars</h1>
    <div class="col-lg-12 h3" id="rover-selected">
      Rover Selected: <span style="color:#c1440e">{{ selected }}</span>
    </div>
  </div>

  <form class="container-fluid" id="main">
    <section class="row">
      <div v-bind:class="validated ? 'col-sm-4' : 'col-sm-6 '">
        <label class="font-weight-bold" for="rover">Rover</label>
        <select required v-model="selected" @change="changeDateMinMax">
          <option disabled value="">Please select one</option>
          <option value="Spirit">Spirit</option>
          <option value="Opportunity">Opportunity</option>
          <option value="Curiosity">Curiosity</option>
        </select>
      </div>

      <div v-bind:class="validated ? 'col-sm-4' : 'col-sm-6'">
        <label class="font-weight-bold" for="pic-date">Date</label>
        <input
          v-model="earthDate"
          type="date"
          id="earth-date"
          name="earth-date"
          :min="minDate"
          :max="maxDate"
          @change="dateValidation"
        /><br />
        <div v-show="invaildDate" class="vaild-date">
          <span class="text-danger">Please enter a invaild date.</span>
        </div>
      </div>
      <div v-bind:class="validated ? 'col-sm-4' : ''">
        <button
          v-show="validated"
          type="button"
          class="btn btn-secondary btn-lg shadow-none button-cameras"
          style="background-color:#c1440e;color:white;margin-top:25px;"
          @click="fetchManifest()"
        >
          Find Cameras
        </button>
      </div>
    </section>

    <div v-if="dateExist" class="gallery">
      <div class="row">
        <div class="col-sm-4" style="margin-top:25px;">
          <h3>TOTAL PHOTOS: {{ totalEarthDayPictures }}</h3>
        </div>
        <div class="col-sm-4">
          <label class="font-weight-bold" for="camera">Cameras </label>
          <select required v-model="camera">
            <option disabled value="">Select one</option>
            <option
              v-for="(camera, index) in earthDateCameras"
              :key="index"
              :value="camera"
            >
              {{ camera }}
            </option>
          </select>
        </div>
        <div class="col-sm-4" style="margin-top:25px;">
          <button
            type="button"
            class="btn btn-secondary btn-lg button-display"
            style="background-color:#e77d11; color:white;"
            @click="fetchCameraPhotos()"
          >
            Display Photos
          </button>
        </div>
      </div>

      <div>
        <section style="margin-top:25px;">
          <div v-if="totalEarthDayPictures > 0" class="container">
            <div
              class="card gallery-item"
              v-for="(photo, index) in earthDatePictures.photos"
              :key="index"
            >
              <img
                class="card-img-top"
                :src="photo.img_src"
                style="border-radius:25px;"
              />
              <div
                class="card-body"
                style="background-color:black; border-radius:25px;"
              >
                <p class="card-text" style="color:white;">
                  {{ photo.camera.full_name }}
                </p>
                <p class="card-text" style="color:white;">
                  Sol: {{ photo.sol }}
                </p>
              </div>
            </div>
          </div>
          <div v-else>
            <h1>
              There are no pictures related to the {{ camera }} camera on the
              {{ earthDate }}
            </h1>
          </div>
        </section>
      </div>
    </div>
    <div v-else class="no-cameras-error">
      <h3 class="text-danger">{{ dateErrors[0] }}</h3>
    </div>
  </form>
</template>

<script>
import axios from "axios";
const API_KEY = process.env.VUE_APP_NASA_API_KEY;

export default {
  name: "Home",
  data() {
    return {
      selected: "",
      camera: "",
      invaildDate: false,
      validated: false,
      earthDate: "",
      minDate: "",
      maxDate: "",
      dateExist: false,
      dateErrors: [],
      roverManifest: [],
      manifestPhotosInfo: [],
      earthDatePictures: [],
      earthDateCameras: [],
      totalEarthDayPictures: 0
    };
  },
  methods: {
    dateValidation() {
      let date = new Date(this.earthDate);

      if (!date && !isNaN(date.getTime())) {
        this.invaildDate = true;
        this.validated = false;
        this.dateExist = false;
      } else if (date.getFullYear() > 2021 || date.getFullYear() < 2004) {
        this.invaildDate = true;
        this.validated = false;
        this.dateExist = false;
      } else {
        this.validated = true;
        this.invaildDate = false;
      }
      return;
    },
    changeDateMinMax() {
      let selectedRover = this.selected;

      if (selectedRover === "Spirit") {
        this.minDate = "2004-01-04";
        this.maxDate = "2010-03-21";
      }

      if (selectedRover === "Curiosity") {
        this.minDate = "2012-08-06";
        this.maxDate = "2021-09-10";
      }

      if (selectedRover === "Opportunity") {
        this.minDate = "2004-01-25";
        this.maxDate = "2018-06-11";
      }
      return;
    },
    extractEarthDateInfo(photosArr) {
      for (let i = 0; i < photosArr.length; i++) {
        let obj = photosArr[i];

        if (obj.earth_date === this.earthDate) {
          this.totalEarthDayPictures = obj.total_photos;
          this.earthDateCameras = obj.cameras;
          this.dateExist = true;
          break;
        } else {
          this.dateExist = false;
          this.dateErrors.push(
            "Please selected another date. There are no cameras availble for the date selected."
          );
        }
      }
      return;
    },
    async fetchManifest() {
      try {
        const url = `https://api.nasa.gov/mars-photos/api/v1/manifests/${this.selected}/?api_key=${API_KEY}`;
        const { data } = await axios.get(url);
        this.roverManifest = data;
        this.manifestPhotosInfo = this.roverManifest.photo_manifest.photos;
        this.extractEarthDateInfo(this.manifestPhotosInfo);
      } catch (err) {
        if (err.response) {
          console.log("Server Error:", err);
        } else if (err.request) {
          console.log("Network Error:", err);
        } else {
          console.log("Client Error:", err);
        }
      }
    },
    async fetchCameraPhotos() {
      try {
        const cameraURL = `https://api.nasa.gov/mars-photos/api/v1/rovers/${this.selected}/photos?earth_date=${this.earthDate}&camera=${this.camera}&page=1&api_key=${API_KEY}`;
        const { data } = await axios.get(cameraURL);
        this.earthDatePictures = data;
      } catch (err) {
        if (err.response) {
          console.log("Server Error:", err);
        } else if (err.request) {
          console.log("Network Error:", err);
        } else {
          console.log("Client Error:", err);
        }
      }
    }
  }
};
</script>

<style>
.container-fluid {
  justify-content: center;
}

span {
  color: #a45d5d;
}

.row {
  font-size: 1.3em;
}

.gallery {
  padding-top: 30px;
}

.container {
  display: flex;
  flex-wrap: wrap;
  padding: 5px;
  justify-content: space-evenly;
}
.gallery-item {
  border-radius: 25px;
  width: 32%;
  margin: 5px;
  margin-bottom: 2%;
}
.no-cameras-error {
  color: red;
  background-color: white;
  margin-top: 60px;
}
label {
  display: block;
  font-weight: bold;
  font-size: 1.5rem;
}

select {
  width: 180px;
  height: 35px;
}
button {
  border: none;
  box-shadow: none;
  border-style: none !important;
}
@media (max-width: 1028px) {
  .container {
    display: flex;
    flex-wrap: wrap;
  }
  .gallery-item {
    border-radius: 25px;
    width: 100%;
    margin: 5px;
    margin-bottom: 2%;
  }
  #title {
    text-align: center;
  }
  #rover-selected {
    text-align: center;
  }
}
</style>
