<template>
  <main id="app" class="d-flex flex-column">

    <header class="bg-dark text-white py-3">
      <div class="container">
        <p class="lead mb-0">
          Use the search box to look up a <a href="https://developers.google.com/places/web-service/intro" target="_blank">Google Place</a>
        </p>
      </div>
    </header>

    <section class="flex-grow-1 bg-white py-5">
      <div class="container">

        <form class="form" @submit.prevent="findCandidates">
          <div class="form-group">
            <div class="input-group input-group-lg">
              <input ref="search" class="form-control" v-model="userInput" placeholder="Search..." aria-label="Search for address candidates" aria-describedby="searchButton" required>
              <div class="input-group-append">
                <button class="btn btn-outline-dark" type="submit" id="searchButton">Search</button>
              </div>
            </div>

            <div v-if="false" class="form-check text-right mt-2">
              <input v-model="showMap" class="form-check-input" type="checkbox" id="showMap">
              <label class="form-check-label" for="showMap">
                Show Map
              </label>
            </div>
          </div>
        </form>

        <div v-show="showMap" is="Map" ref="googleMap"></div>

        <div v-if="results.length" class="list-group list-group-flush">
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
    </section>
  </main>
</template>

<script>
import Map from './components/Map'

export default {
  components: { Map },
  data () {
    return {
      showMap: false,
      userInput: null,
      results: [],
      service: null
    }
  },
  mounted () {
    this.$refs.search.focus()
    this.service = new google.maps.places.PlacesService(this.$refs.googleMap.map)
  },
  methods: {
    findCandidates (e) {
      this.results = []

      let request = {
        query: this.userInput,
        fields: ['name', 'formatted_address', 'name', 'place_id', 'geometry'],
      }

      this.service.findPlaceFromQuery(request, this.callback)
    },
    callback (results, status) {
      if (status == google.maps.places.PlacesServiceStatus.OK) {
        this.results = results
      }
    },
    selectPlace (place) {
      window.top.returnValue = JSON.stringify(place)
      window.top.dialogClose()
    }
  }
}
</script>

<style src="./assets/main.css"></style>
