<template>
  <div id="app" class="bg-white py-5">
    <div class="container">

      <form class="form" @submit.prevent="findCandidates">
        <div class="form-group">
          <div class="input-group input-group-lg">
            <input class="form-control" v-model="userInput" placeholder="Search..." aria-label="Search for address candidates" aria-describedby="searchButton" required>
            <div class="input-group-append">
              <button class="btn btn-outline-secondary" type="submit" id="searchButton">Search</button>
            </div>
          </div>
        </div>
      </form>

      <div v-if="showMap" class="embed-responsive embed-responsive-16by9">
        <div id="map" ref="map" class="embed-responsive-item"></div>
      </div>

      <div v-if="results.length" class="list-group">
        <a v-for="p in results" href="#" class="list-group-item list-group-item-action" @click.prevent="selectPlace(p)">
          {{ p.name }}
          <div class="d-flex justify-content-between small text-muted">
            <span>
              {{ p.formatted_address }}
            </span>
            <span>
              {{ p.place_id }}
            </span>
          </div>
        </a>
      </div>

      <div v-else class="bg-light text-center p-3 h4">
        No Results
      </div>

      <div class="small text-right mt-3">
        <a href="https://support.google.com/maps/answer/6320846" target="_blank">
          Add a place to google maps
        </a>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      showMap: false,
      userInput: null,
      results: [],
      service: null
    }
  },
  mounted () {
    this.service = new google.maps.places.PlacesService(document.createElement('div'))
  },
  methods: {
    findCandidates (e) {
      console.log('findCandidates');
      this.results = []
      this.service.findPlaceFromQuery({
        query: this.userInput,
        fields: ['name', 'formatted_address', 'name', 'place_id', 'geometry'],
      }, this.callback)
    },
    callback (results, status) {
      if (status == google.maps.places.PlacesServiceStatus.OK) {
        this.results = results
      }
    },
    selectPlace (place) {
      console.log('place', place)
    }
  }
}
</script>
