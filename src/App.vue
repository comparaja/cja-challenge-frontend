<template>
  <div id="app">
    <Navbar></Navbar>
    <div class="banner">
      <h1>Resultados TV-Net-Voz</h1>
      <p>Encontrámos {{ results.length }} resultado{{ results.length != 1 ? 's' : ''}} indicados para si.</p>
    </div>
    <Filters v-model="filter" :providers="providers" :connectionTypes="connectionTypes" ></Filters>
    <div class="results">
      <Result v-for="result in results" :key="result.id" :result="result"></Result>
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import Filters from './components/Filters.vue'
import Result from './components/Result.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    Filters,
    Result
  },
  data() {
    return {
      results: [],
      providers: [],
      connectionTypes: [],
      filter: {
        provider: '',
        connectionType: '',
        channels: "30",
        internetSpeed: "200",
        price: "40",
        sortBy: 'monthlyPrice'
      }
    }
  },
  watch: {
    filter: {
      deep: true,
      handler(val) {
        this.getResults()
      }
    }
  },
  async mounted() {
    await this.fetchProviders()
    await this.fetchConnectionTypes()
    this.getResults()
  },
  methods: {
    async getResults() {
      this.results = []
      let request = '?'

      if(this.filter.provider) {
        request += "&providerId=" + this.filter.provider
      }

      if(this.filter.connectionType) {
        request += "&connectionTypeId=" + Number(this.filter.connectionType)
      }

      if(this.filter.sortBy) {
        let dir = 'asc'
        if(this.filter.sortBy != 'monthlyPrice') {
          dir = 'desc'
        }
        request += "&sortBy=" + this.filter.sortBy + "&sortDirection=" + dir
      } else {
        request += "&sortBy=monthlyPrice&sortDirection=asc"
        this.filter.sortBy = "monthlyPrice"
      }

      await fetch('http://localhost:8080/api/v1/products/list' + request, {
        isActive: true,
        providerId: this.filter.provider,
        connectionTypeId: this.filter.connectionType
      }).then((response) => {
            return response.json()
          }).then((data) => {
            data.forEach((el) => {
            this.results.push({
              id: el.id,
              numberOfChannels: el.numberOfChannels,
              monthlyPrice: el.monthlyPrice,
              description: el.description,
              internetSpeed: el.internetSpeed,
              isActive: el.isActive,
              provider: this.providers.filter(it => {
                return it.id == el.providerId
              })[0].name,
              connectionType: this.connectionTypes.filter(it => {
                return it.id == el.connectionTypeId
              })[0].description,
            })
          })
          })
    },
    async fetchProviders() {
      await fetch('http://localhost:8080/api/v1/provider/list?isActive=true').then((response) => {
          return response.json()
        }).then((data) => {
          this.providers = data
        })
    },
    async fetchConnectionTypes() {
      await fetch('http://localhost:8080/api/v1/connectionType/list?isActive=true').then((response) => {
          return response.json()
        }).then((data) => {
          this.connectionTypes = data
        })
    },
  }
}
</script>

<style>
@import './assets/colors.css';

html {
  background-color: var(--white3);
}

body {
  margin: 0;
}

#app {
  font-family: Montserrat, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--grey2);
}

.banner {
  background-color: var(--blue1);
  color: var(--white);
  padding: 20px calc((100vw - 1020px) / 2);
}

.results {
  padding: 20px calc((100vw - 1020px) / 2);
}

.results > * {
  margin-bottom: 20px;
}


@media ( max-width: 1200px) {
  .banner, .results {
    padding: 20px;
  }
}
</style>
