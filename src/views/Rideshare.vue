<template>
  <div class="rideshare">
    <div class="container">
      <div class="row">
        <div class="col-xl-4 col-lg-5">
          <div class="card card-primary animated fadeInUp animation-delay-7">
            <div class="card-body">
              <form class="form-horizontal" v-on:submit.prevent="newQuill()">
                <fieldset class="container">
                  <div class="form-group row justify-content-end">
                    <div class="form-group row justify-content-end">
                      <h3>Select a neighborhood:</h3>
                      <label for="select112" class="col-lg-2 control-label"></label>
                      <div class="col-lg-10">
                        <select
                          id="select112"
                          class="form-control selectpicker"
                          data-live-search="true"
                          data-dropup-auto="false"
                          v-model="commarea_full_selection"
                        >
                          <option>1 - Rogers Park</option>
                          <option>2 - West Ridge</option>
                          <option>3 - Uptown</option>
                          <option>4 - Lincoln Square</option>
                          <option>5 - North Center</option>
                        </select>
                      </div>
                    </div>
                    <label class="col-lg-2 control-label justify-content-end"></label>
                    <div class="col-lg-10">
                      <div class="radio radio-primary">
                        <h3>Persona:</h3>
                        <label>
                          <input type="radio" name="optionsRadios" v-model="persona" value="driver" checked="" />
                          Driver
                        </label>
                      </div>
                      <div class="radio radio-primary">
                        <label>
                          <input type="radio" name="optionsRadios" v-model="persona" value="passenger" />
                          Passenger
                        </label>
                      </div>
                    </div>
                  </div>

                  <div class="form-group row justify-content-end">
                    <label class="col-lg-2 control-label"></label>
                    <div class="col-lg-10">
                      <div class="radio radio-primary">
                        <h3>Ride Type:</h3>
                        <label>
                          <input type="radio" name="optionsRadios" v-model="ride_type" value="individual" checked="" />
                          Individual
                        </label>
                      </div>
                      <div class="radio radio-primary">
                        <label>
                          <input type="radio" name="optionsRadios" v-model="ride_type" value="shared" />
                          Shared
                        </label>
                      </div>
                    </div>
                  </div>
                  <div class="form-group row justify-content-end">
                    <div class="col-lg-10">
                      <button type="submit" class="btn btn-raised btn-primary">Quill it!</button>
                    </div>
                  </div>
                </fieldset>
              </form>
            </div>
          </div>
        </div>
        <div class="col-xl-8 col-lg-7">
          <div class="card card-primary animated fadeInUp animation-delay-7">
            <div class="ms-hero-bg-primary ms-hero-img-mountain">
              <h2 class="text-center no-m pt-4 pb-4 color-white index-1">Quill Content</h2>
            </div>
            <div class="card-body" id="quill-content">
              <p>{{ quill_response }}</p>
            </div>
          </div>
          <div class="card card-primary animated fadeInUp animation-delay-7" id="google-maps-holder">
            <div class="card-header">
              <h3 class="card-title">
                <i class="zmdi zmdi-map"></i>
                Map
              </h3>
            </div>
            <div id="google-map-div">
              <img
                id="google-map"
                src="https://maps.googleapis.com/maps/api/staticmap?center=Chicago&zoom=14&size=730x380&key=AIzaSyD_1TkqGPWSc7UueUNPXDQLuNvUFuygIok"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- container -->
  </div>
</template>

<script>
// @ is an alias to /src
var axios = require("axios");

export default {
  data: function() {
    return {
      commarea_full_selection: "",
      persona: "",
      ride_type: "",
      commarea_number: "",
      commarea_prettyname: "",
      quill_response: "You haven't submitted anything yet..."
    };
  },
  methods: {
    newQuill: function() {
      let commarea_number = this.commarea_full_selection.split("-")[0].trim();
      let commarea_prettyname = this.commarea_full_selection.split("-")[1].trim();
      this.commarea_prettyname = commarea_prettyname;
      this.errors = [];

      let limit = 10000;

      // TODO: add in date filter for TNP data
      let tnp_pickup_url =
        encodeURI("https://data.cityofchicago.org/resource/m6dm-c72p.json?$$app_token=uQLbXrRXncBl4YeOvSLny1tNW&$limit=" + limit + "&$where=pickup_community_area=" +
        commarea_number + " AND dropoff_community_area !=" + commarea_number);
      console.log("All pickups, no dropoffs:\n " + tnp_pickup_url);

      // Mike June 4th, removing dropoffs since they're not necessary
      // let tnp_dropoff_url =
      //   encodeURI("https://data.cityofchicago.org/resource/m6dm-c72p.json?$$app_token=uQLbXrRXncBl4YeOvSLny1tNW&$limit=" + limit + "&dropoff_community_area=" + commarea_number + " AND pickup_community_area !=" + commarea_number);
      // console.log(tnp_dropoff_url);

      let tnp_pickup_and_dropoff_intersection_url =
        encodeURI("https://data.cityofchicago.org/resource/m6dm-c72p.json?$$app_token=uQLbXrRXncBl4YeOvSLny1tNW&$limit=" + limit + "&dropoff_community_area=" + commarea_number + "&pickup_community_area=" + commarea_number);
      console.log("All pickups AND dropoffs:\n" + tnp_pickup_and_dropoff_intersection_url)

      // TNP API calls
      axios
        .get(tnp_pickup_url)
        .then(response => {
          let tnp_pickups_response = response.data;

          // Mike 6/4 removing this as part of removing dropoffs URL above
          // axios.get(tnp_dropoff_url).then(response => {
          //   let tnp_dropoffs_response = response.data;

            axios.get(tnp_pickup_and_dropoff_intersection_url).then(response => {
              let tnp_pick_and_drop_response = response.data;


              // Now that we have both TNP results, concat themt ogether into final JSON spec for Quill
              // Also add in metadata from form
              var json_output_to_quill = {
                persona: this.persona,
                ride_type: this.ride_type,
                neighborhood: this.commarea_prettyname,

                // Mike 6/4 removing this as part of removing dropoffs URL above
                // trips: tnp_pickups_response.concat(tnp_dropoffs_response).concat(tnp_pick_and_drop_response)
                trips: tnp_pickups_response.concat(tnp_pick_and_drop_response)
              };

              console.log("JSON output to Quill:\n\n");
              console.log(json_output_to_quill)

              let axiosConfig = {
                headers: {
                  "content-type": "application/json",
                  "x-ns-accepts": "html",
                  "x-ns-api-token": "5cd1b5d935a4462969042285",
                  "x-ns-template": "5cf53c147a2618415718eaab"
                }
              };
              // Now send the data spec to Quill API
              axios
                .post(
                  "https://api.narrativescience.com/v4/press/5cd1b61f35a4464c554c7023/story/",
                  json_output_to_quill,
                  axiosConfig
                )
                .then(response => {
                  this.quill_response = response.data;
                  console.log("quill_response:\n\n" + this.quill_response);
                });
              });
          // });
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });

      // Google Maps call for pretty name neighborhood
      let map_url =
        "https://maps.googleapis.com/maps/api/staticmap?center=" +
        encodeURI(commarea_prettyname) +
        ",Chicago&zoom=15&size=730x380&key=AIzaSyD_1TkqGPWSc7UueUNPXDQLuNvUFuygIok";
      console.log(map_url);
      document.getElementById("google-map").src = map_url;
    }
  }
};
</script>
