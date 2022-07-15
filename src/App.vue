<template>
  <div>
    <div>
      <!--      <input type="button" value="刷新数据">-->
    </div>
    <p>数量: {{ items.length }}</p>
    <p>最新发售时间</p>
    <ul>
      <li v-for="row, i in this.latestItemTimes" :key="i">{{ row }}</li>
    </ul>
    <p>详细列表</p>
    <table>
      <tr>
        <th>#</th>
        <th>id</th>
        <th>chain</th>
        <th>碎片</th>
        <th>MCN</th>
        <th>次数</th>
        <th>时间</th>
      </tr>
      <tr v-for="item, i in this.items" :key="i">
        <td>{{ i }}</td>
        <td>{{ item.good_id }}</td>
        <td>{{ item.chain_id }}</td>
        <td>{{ parseInt(item.sui_price) }}</td>
        <td>{{ parseInt(item.mcn_price) }}</td>
        <td>{{ item.count }}</td>
        <td>{{ item.start_time }}</td>
      </tr>
    </table>
  </div>

</template>

<script>
const axios = require('axios');

export default {
  name: 'App',
  components: {},
  data() {
    return {
      items: []
    }
  },
  computed: {
    latestItemTimes() {
      let latestGoodTimes = this.items.map(x => x.start_time).sort().reverse().slice(0, 5)
      return latestGoodTimes
    }
  },
  methods: {
    updateItems() {
      const goodUrl = "https://www.meta-studios.io/index/index/getGoodList"
      let res = axios.get(goodUrl)
          .then(res => res.data)
          .then(data => this.items = data.data)
      return res
    }
  },
  mounted() {
    this.updateItems()
    setInterval(() => {
      this.updateItems()
    }, 10000)
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

table {
  border: 1px solid #ccc;
  text-align: left;
}
th td {
  padding: 10px;
}
</style>
