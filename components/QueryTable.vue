<template>
  <div>
    <div v-if="header.length === 0 && !initial" class="error-text">No Result</div>
    <table v-else>
      <tr>
        <th v-for="(h, idx) in header" :key="idx">{{ h }}</th>
      </tr>
      <tr v-for="(row, idx) in rows" :key="idx">
        <td v-for="(col, key) in row" :key="key">{{ col }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    initial: {
      type: Boolean,
      default: false
    },
    header: {
      type: Array,
      default: () => []
    },
    rows: {
      type: Array,
      default: () => []
    },
    query: {
      type: String,
      default: ''
    }
  }
}
</script>

<style>
.error-text {
  text-align: center;
  font-size: 22px;
  font-weight: 500;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #000000;
  text-align: left;
  padding: 8px;
  font-size: 12px;
}

tr {
  width: 100%;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
