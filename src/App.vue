<template>
  <div id="app">
    <Loader :loading="showLoader" />
    <h2>Vue Pagination</h2>
    <ul class="flex-container">
      <li v-for="item in listItems" :key="item.id" class="flex-item">
        <h4>Passenger: {{item.name}}</h4>
        <h4>Airline: {{item.airline[0] && item.airline[0].name}}</h4>
        <h4>Country: {{item.airline[0] && item.airline[0].country}}</h4>
        <img :src="item.airline[0] && item.airline[0].logo" />
      </li>
      <li v-if="listItems.length === 0" class="flex-item center">No Record Found</li>
    </ul>
    <ul class="showItems">
      <li>Show Items:
        <select @change="onChangeRecordsPerPage" v-model="recordsPerPage">
          <option :value="10">10</option>
          <option :value="20">20</option>
          <option :value="30">30</option>
          <option :value="40">40</option>
          <option :value="50">50</option>
        </select>
      </li>
      <li>
        Go to Page <input type="text" v-model="enterpageno"><button type="button" @click.prevent="gotoPage">Go</button>
      </li>
    </ul>
    <Pagination v-if="listItems" :total-pages="totalPages" :per-page="recordsPerPage" :current-page="page" @pagechanged="onPageChange" />
  </div>
</template>

<script>
import axios from 'axios'
import Pagination from './components/Pagination'
import Loader from './components/Loader'
import { baseApiURL } from '@/config/env'

export default {
  components: {
    Pagination,
    Loader
  },
  data () {
    return {
      showLoader: false,
      listItems: [],
      page: 1,
      totalPages: 0,
      totalRecords: 0,
      recordsPerPage: 10,
      enterpageno: ''
    }
  },
  created () {
    this.loadListItem()
  },
  methods: {
    loadListItem () {
      this.showLoader = true
      axios.get(`${baseApiURL}/v1/passenger?page=${this.page}&size=${this.recordsPerPage}`)
        .then(response => {
          console.log('response', response)
          this.showLoader = false
          this.listItems = response.data.data
          this.totalPages = Math.floor(response.data.totalPassengers / this.recordsPerPage) // Calculate total records
          this.totalRecords = response.data.totalPassengers
        })
    },
    onPageChange (page) {
      this.page = page
      this.loadListItem()
    },
    onChangeRecordsPerPage () {
      this.loadListItem()
    },
    gotoPage () {
      if (!isNaN(parseInt(this.enterpageno))) {
        this.page = parseInt(this.enterpageno)
        this.loadListItem()
      }
    }
  }
}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h2 {
  text-align: center;
}
ul li {
  list-style-type: none;
}
ul.flex-container {
  padding: 0;
  margin: 0;
  list-style-type: none;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-flex-flow: row wrap;
  flex-direction: row wrap;
  flex-wrap: wrap;
  justify-content: space-around;
  .flex-item {
    background: tomato;
    width: calc(100% / 5.5);
    padding: 5px;
    height: auto;
    margin-top: 10px;
    color: white;
    font-weight: bold;
    text-align: center;
  }
  img {
    display: initial;
    width: 200px;
  }
}
.showItems {
  display: inline-block;
  margin-left: -35px;
  li {
    list-style-type: none;
    display: inline-block;
    margin-left: 10px;
  }
}
</style>
