<template>
  <div class="hello">
    <input v-model="value" type="text" class="border-2 border-gray-500 text-center shadow-lg rounded-lg mb-8" />
    <div class="cards grid grid-cols-3 gap-4">
      <div v-for="result of results" :key="result._id" class="card rounded overflow-hidden shadow-lg">
        <img class="w-full" :src="'file:///C:/Users/t/Desktop/Images/' + result._source.image" :alt="result._source.caption">
        <div class="px-6 py-4">
          <p class="text-gray-700 text-base">
            {{ result._source.caption }}
          </p>
        </div>
        <div class="px-6 pt-4 pb-2">
          <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{ result._score }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import debounce from "lodash.debounce";
import axios from "axios";
export default {
  name: 'SearchPage',
  props: {
    msg: String
  },
  data() {
    return {
      value: "",
      results: []
    }
  },
  watch: {
    value(...args) {
      this.debouncedWatch(...args);
    },
  },
  created() {
    // Watch for updates in input field
    // eslint-disable-next-line no-unused-vars
    this.debouncedWatch = debounce(async (newValue, _) => {
      try {
        const res = await axios({
          method: 'post',
          url: 'http://localhost:9200/test_dataset/_search?size=100',
          data: {
            track_total_hits: true,
            query: {match: {caption: newValue}}
          }
        })
        this.results = res.data.hits.hits
        console.log('New value:', res.data.hits.hits);
      } catch (e) {
        console.log('Error in Elasticsearch req');
      }
    }, 300);
  },
  beforeUnmount() {
    this.debouncedWatch.cancel();
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  background-color: rgb(255,255,255);
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
