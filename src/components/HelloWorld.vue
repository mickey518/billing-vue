
<script>

import axios from 'axios'
// Import LightningChartJS
const lcjs = require('@arction/lcjs')
const {
  lightningChart,
  PieChartTypes,
  LegendBoxBuilders,
  SliceLabelFormatters,
  Themes
} = lcjs

const pieType = window.innerWidth > 599 ? PieChartTypes.LabelsOnSides : PieChartTypes.LabelsInsideSlices

const pie = lightningChart().Pie({
  theme: Themes.lightNew,
  type: pieType
})
  .setTitle('Project Time Division')
  .setAnimationsEnabled(true)
  .setMultipleSliceExplosion(true)

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      totalOfYear: {}
    }
  },
  methods: {
    loadTotalYear () {
      axios.get('/api/pay/wechat/total/year')
        .then(response => {
          this.totalOfYear = response.data

          const slices = this.totalOfYear.map((item) => 
          {
            console.log(item.year.toString())
            pie.addSlice({ name: item.year.toString(), value: item.money })
          }
          )
          console.log(this.totalOfYear)
          // Specify function which generates text for Slice Labels(LabelFormatter).

          // pie.setLabelFormatter(SliceLabelFormatters.NamePlusRelativeValue)
          // ----- Add LegendBox -----
          pie.addLegendBox(LegendBoxBuilders.VerticalLegendBox)
            // Dispose example UI elements automatically if they take too much space. This is to avoid bad UI on mobile / etc. devices.
            .setAutoDispose({
              type: 'max-width',
              maxWidth: 0.30
            })
            .add(pie)
        })
        .catch(function (error) { // 请求失败处理
          console.log(error)
        })
    }
  },
  mounted () {
    this.loadTotalYear()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
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
