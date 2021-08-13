
<template>
  <div class="home">
    <div class="search-box">
      <textarea id="query" v-model="query" placeholder="SELECT orderID,customerID FROM orders" name="query" cols="60" rows="3"></textarea>
      <button @click="runQuery">Run Query</button>
    </div>
    <QueryTable :header="header" :initial="initial" :rows="result" query="query" />
  </div>
</template>

<script>
import QueryTable from '@/components/QueryTable.vue'

const baseURL = 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/bc7f69ab36a81265d07652f22ddd3377ee69b412/examples/northwind/data/csv/'

export default {
  name: 'Home',
  components: {
    QueryTable
  },
  data() {
    return {
      csvData: '',
      header: [],
      result: [],
      query: 'SELECT * FROM orders',
      initial: true
    }
  },
  mounted() {
    this.runQuery()
  },
  methods: {
    csvToArray(columns) {
      const lines = this.csvData.split('\n')
      const result = []

      const newHeader  = lines[0].split(',') // select orderID, order from orders

      this.header = newHeader

      if (columns !== '*') {
        columns = columns.split(',')
        this.header = []
        columns.forEach(c => {
          if (newHeader.includes(c)) {
            this.header.push(c)
          }
        })
      }

      for (let i = 1; i < lines.length; i++) {
        if (!lines[i]) {
          continue
        }
        const csvRow = {}
        const currentLine = lines[i].split(',')

        for (let j = 0; j < this.header.length; j++) {
          csvRow[this.header[j]] = currentLine[j]
        }

        result.push(csvRow)
      }

      this.result = result
    },
    async runQuery() {
      const queryArray = this.query.trim().split(' ')
      // console.log(queryArray)
      if (queryArray.length !== 4) {
        return
      }

      const columns = queryArray[1]
      const table = queryArray[3]

      try {
        const response = await this.$axios.$get(`${baseURL+table}.csv`)
        this.csvData = response
        this.csvToArray(columns)
        this.initial = false
      } catch (e) {
        this.header = []
        this.initial = false
      }
        
    }
  }
}
</script>

<style scoped>
.search-box {
  display: flex;
  width: 100%;
  align-items: flex-end;
  justify-content: center;
  margin: 20px auto;
}
textarea {
  border: 1px solid black;
  display: inline-block;
  padding: 20px;
  margin-right: 20px;
  background-color: #eeeeee;
}

button {
  background-color: #e4e4e4;
  padding: 8px 16px;
  border-radius: 6px;
  align-self: flex-end;
}
</style>