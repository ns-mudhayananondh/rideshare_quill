<template>
  <div class="rideshare">
    <h1></h1>
    <div class="container">
      <div class="row">
        <div class="col-xl-4 col-lg-5">
          <div class="card card-primary animated fadeInUp animation-delay-7">
            <div class="card-body">
              <form class="form-horizontal" v-on:submit.prevent="newQuill()">
                <fieldset class="container">
                  <div>
                    <div class="form-group row">
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
                          <option v-for="dropdown_value in dropdown_values">
                            {{ dropdown_value.id }} - {{ dropdown_value.name }}
                          </option>
                        </select>
                      </div>
                    </div>
                    <div class="col-lg-10">
                      <div class="radio radio-primary">
                        <h3>Persona:</h3>
                        <label>
                          <input type="radio" name="optionsRadios" v-model="persona" value="driver" checked="" />
                          Driver
                        </label>
                        <label>
                          <input type="radio" name="optionsRadios" v-model="persona" value="passenger" />
                          Passenger
                        </label>
                      </div>
                    </div>
                    <div class="radio radio-primary">
                      <h3>Timeslots:</h3>
                      <label>
                        <input type="radio" name="optionsRadios2" v-model="time" value="morning" checked="" />
                        Morning: 06:00 - 11:59
                      </label>
                      <label>
                        <input type="radio" name="optionsRadios2" v-model="time" value="afternoon" checked="" />
                        Afternoon: 12:00 - 17:59
                      </label>
                      <label>
                        <input type="radio" name="optionsRadios2" v-model="time" value="evening" checked="" />
                        Evening: 18:00 - 23:59
                      </label>
                      <label>
                        <input type="radio" name="optionsRadios2" v-model="time" value="late_night" checked="" />
                        Late Night: 00:00 - 5:59
                      </label>
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
              <span v-html="quill_response"></span>
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
      // Mike 6/4 harcoding this as "morning" for now, see Mavenlink
      // for all four possible string values
      time: "",
      quill_response: "You haven't submitted anything yet...",
      dropdown_values: [
        { id: 1, name: "Rogers Park" },
        { id: 2, name: "West Ridge" },
        { id: 3, name: "Uptown" },
        { id: 4, name: "Lincoln Square" },
        { id: 5, name: "North Center" },
        { id: 6, name: "Lake West" },
        { id: 7, name: "Lincoln Park" },
        { id: 8, name: "Near North Side" },
        { id: 9, name: "Edison Park" },
        { id: 10, name: "Norwood Park" },
        { id: 11, name: "Jefferson Park" },
        { id: 12, name: "Forest Glen" },
        { id: 13, name: "North Park" },
        { id: 14, name: "Albany Park" },
        { id: 15, name: "Portage Park" },
        { id: 16, name: "Irving Park" },
        { id: 17, name: "Dunning" },
        { id: 18, name: "Montclare" },
        { id: 19, name: "Belmont Cragin" },
        { id: 20, name: "Hermosa" },
        { id: 21, name: "Avondale" },
        { id: 22, name: "Logan Square" },
        { id: 23, name: "Humboldt Park" },
        { id: 24, name: "West Town" },
        { id: 25, name: "Austin" },
        { id: 26, name: "West Garfield Park" },
        { id: 27, name: "East Garfield Park" },
        { id: 28, name: "Near West Side" },
        { id: 29, name: "North Lawndale" },
        { id: 30, name: "South Lawndale" },
        { id: 31, name: "Lower West Side" },
        { id: 32, name: "Loop" },
        { id: 33, name: "Near South Side" },
        { id: 34, name: "Armour Square" },
        { id: 35, name: "Douglas" },
        { id: 36, name: "Oakland" },
        { id: 37, name: "Fuller Park" },
        { id: 38, name: "Grand Boulevard" },
        { id: 39, name: "Kenwood" },
        { id: 40, name: "Washington Park" },
        { id: 41, name: "Hyde Park" },
        { id: 42, name: "Woodlawn" },
        { id: 43, name: "South Shore" },
        { id: 44, name: "Chatham" },
        { id: 45, name: "Avalon Park" },
        { id: 46, name: "South Chicago" },
        { id: 47, name: "Burnside" },
        { id: 48, name: "Calumet Heights" },
        { id: 49, name: "Roseland" },
        { id: 50, name: "Pullman" },
        { id: 51, name: "South Deering" },
        { id: 52, name: "East Side" },
        { id: 53, name: "West Pullman" },
        { id: 54, name: "Riverdale" },
        { id: 55, name: "Hegewisch" },
        { id: 56, name: "Garfield Ridge" },
        { id: 57, name: "Archer Heights" },
        { id: 58, name: "Brighton Park" },
        { id: 59, name: "McKinley Park" },
        { id: 60, name: "Bridgeport" },
        { id: 61, name: "New City" },
        { id: 62, name: "West Elsdon" },
        { id: 63, name: "Gage Park" },
        { id: 64, name: "Clearing" },
        { id: 65, name: "West Lawn" },
        { id: 66, name: "Chicago Lawn" },
        { id: 67, name: "West Englewood" },
        { id: 68, name: "Englewood" },
        { id: 69, name: "Greater Grand Crossing" },
        { id: 70, name: "Ashburn" },
        { id: 71, name: "Auburn Gresham" },
        { id: 72, name: "Beverly" },
        { id: 73, name: "Washington Heights" },
        { id: 74, name: "Mount Greenwood" },
        { id: 75, name: "Morgan Park" },
        { id: 76, name: "O'Hare" },
        { id: 77, name: "Edgewater" }
      ]
    };
  },
  methods: {
    newQuill: function() {
      let commarea_number = this.commarea_full_selection.split("-")[0].trim();
      let commarea_prettyname = this.commarea_full_selection.split("-")[1].trim();
      this.commarea_prettyname = commarea_prettyname;
      this.errors = [];

      let limit = 10000;

      // Mike 6/4 hardcoded "time" filter hours for now, can implement
      // similar to the commarea parsing once UI element built
      let tnp_pickup_url = encodeURI(
        "https://data.cityofchicago.org/resource/m6dm-c72p.json?$$app_token=uQLbXrRXncBl4YeOvSLny1tNW" +
          "&$limit=" +
          limit +
          "&$where=pickup_community_area=" +
          commarea_number +
          " AND dropoff_community_area !=" +
          commarea_number +
          " AND date_extract_hh(trip_start_timestamp) >= " +
          6 +
          " AND date_extract_hh(trip_start_timestamp) < " +
          12
      );

      console.log("All pickups, no dropoffs:\n\n" + tnp_pickup_url);

      // Mike June 4th, removing dropoffs since they're not necessary
      // let tnp_dropoff_url =
      //   encodeURI("https://data.cityofchicago.org/resource/m6dm-c72p.json?$$app_token=uQLbXrRXncBl4YeOvSLny1tNW&$limit=" + limit + "&dropoff_community_area=" + commarea_number + " AND pickup_community_area !=" + commarea_number);
      // console.log(tnp_dropoff_url);

      let tnp_pickup_and_dropoff_intersection_url = encodeURI(
        "https://data.cityofchicago.org/resource/m6dm-c72p.json?$$app_token=uQLbXrRXncBl4YeOvSLny1tNW" +
          "&$limit=" +
          limit +
          "&dropoff_community_area=" +
          commarea_number +
          "&pickup_community_area=" +
          commarea_number +
          "&$where=date_extract_hh(trip_start_timestamp) >= " +
          6 +
          " AND date_extract_hh(trip_start_timestamp) < " +
          12
      );

      console.log("All pickups AND dropoffs:\n\n" + tnp_pickup_and_dropoff_intersection_url);

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
              time: this.time,
              // Mike 6/4 removing this as part of removing dropoffs URL above
              // trips: tnp_pickups_response.concat(tnp_dropoffs_response).concat(tnp_pick_and_drop_response)
              trips: tnp_pickups_response.concat(tnp_pick_and_drop_response)
            };

            console.log("JSON output to Quill:\n\n");
            console.log(json_output_to_quill);

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
