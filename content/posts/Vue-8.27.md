---
title: "echart、导出、弹窗组件"
date: 2020-08-27T14:44:51+08:00
categories: ["编程 · 技术"]
tags: ["Vue"]
draft: false
---

## echart图表相关

把echart表弄成组件，方便管理

```vue
<template>
  <div>
    <div class="dashboard-container">
      <puv-chart :chart-data="chartData" />
      <el-row class="mt">
        <el-col :span="12">
        	//equ-chart =equChart
          <equ-chart :chart-data="equipmentData" />
        </el-col>
        <el-col :span="12">
          <ori-chart :chart-data="originData" />
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
//导出插件
import puvChart from './components/puv'
import equChart from './components/equipment'
import oriChart from './components/origin'
import dateFormat from '@/utils/dateFormat'

export default {
  name: 'Dashboard',
  components: {
    puvChart,
    equChart,
    oriChart
  },
  data() {
    return {
      chartData: {},
      equipmentData: {},
      originData: {},
      oriTable: []
    }
  },
  methods: {
    // 接口
    setDate() {
      const _query = JSON.parse(JSON.stringify(this.query))
      _query.page = this.paging.page
      _query.page_size = this.paging.page_size
      for (const key in _query) {
        !_query[key] && delete _query[key]
      }

      // 折线图
      this.$api.origin.GetPvUv(_query).then(res => {
        this.chartData = res.data.data
      })
      // 访问设备
      this.$api.origin.GetUA(_query).then(res => {
        this.equipmentData = res.data.data
        if (this.equipmentData.ua.length === 0) {
          this.tipChart = true
        }
      })
      // 访问来源
      this.$api.origin.GetOriginSort(_query).then(res => {
        this.originData = res.data.data
      })
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
```

eChart表---折线图

```vue
<template>
  <div class="charts-cont">
      <!-- 数据显示 -->
    <div class="charts-wrap">
      <div id="countChart" class="charts" />
    </div>
  </div>
</template>

<script>
import echarts from 'echarts'

export default {
  name: 'CountChart',
  props: {
    chartData: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      countChart: null, // 图表对象
      chartOption: {
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['访客量', '浏览量']
        },
        xAxis: {
          type: 'category',
          boundaryGap: false, // 从原点开始绘制
          data: []
        },
        yAxis: {},
        dataZoom: [{
          type: 'inside',
          start: 0,
          end: 100
        }],
        series: [
          {
            name: '访客量',
            type: 'line',
            stack: 'uv',
            data: []
          },
          {
            name: '浏览量',
            type: 'line',
            stack: 'pv',
            data: []
          }
        ]
      }
    }
  },
  watch: {
    chartOption(n, o) {
      this.setOption(n)
    },
    chartData() {
      this.setData()
      this.render()
    }
  },
  created() {

  },
  methods: {
    init() {
      this.setData()
      this.render()
      window.addEventListener('resize', () => {
        this.countChart.resize()
      })
    },
    setData() {
      //  源数据
      const _uvArr = this.chartData.uv
      const _pvArr = this.chartData.pv

      //  图表数据
      const _xAxis = []
      const _seriesDataUV = []
      const _seriesDataPV = []

      //  重组x时间轴数据
      _uvArr.forEach((item, index) => {
        _xAxis.push(item.date)
        _seriesDataUV.push(item.count)
        _seriesDataPV.push(_pvArr[index].count) // 因为pv和uv数组长度相对应，所以直接用索引
      })
      this.chartOption.xAxis.data = _xAxis
      this.chartOption.series[0].data = _seriesDataUV
      this.chartOption.series[1].data = _seriesDataPV
    },
    render() {
      this.countChart = echarts.init(document.getElementById('countChart'))
      this.setOption(this.chartOption)
    },
    setOption(options) {
      this.countChart.setOption(options)
    }
  }
}
</script>

<style lang="scss" scoped>
.charts {
  height: 500px;
}
</style>
```

echart---饼图

```vue
<template>
  <div class="charts-cont">
    <div class="total-cont">
      <!-- 数据显示 -->
      <div class="charts-wrap">
        <div id="originChart" class="equipment" />
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import echarts from 'echarts'

export default {
  name: 'OriginChart',
  props: {
    chartData: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      originChart: null, // 图表对象
      origionOption: {
        title: {
          text: '访问来源',
          left: 'left'
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        legend: {
          orient: 'vertical',
          data: [],
          right: '20%',
          top: 120
        },
        series: [
          {
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            center: ['30%', '60%'],
            data: [],
            label: {
              show: false
            },
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  watch: {
    origionOption(n, o) {
      this.setOption(n)
    },
    chartData(n, o) {
      if (!n.groups) return false
      this.setData()
      this.render()
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      if (this.chartData.groups) {
        this.setData()
        this.render()
      }
      window.addEventListener('resize', () => {
        this.originChart.resize()
      })
    },
    setData() {
      //  源数据
      const _data = this.chartData.groups

      // 图表数据
      const _legend = []
      const _series = []

      // 重组
      _data.forEach(item => {
        _legend.push(item.name)
        _series.push({ value: parseInt(item.count), name: item.name })
      })
      this.origionOption.legend.data = _legend
      this.origionOption.series[0].data = _series
    },
    render() {
      this.originChart = echarts.init(document.getElementById('originChart'))
      this.originChart.setOption(this.origionOption)
    },
    setOption(options) {
      this.originChart.setOption(options)
    }
  }
}
</script>

<style lang="scss" scoped>
.equipment{
  height: 300px;
  margin: 0 auto;
  padding: 0 10px;
 }
</style>
```

## 导出

```js
const _query = JSON.parse(JSON.stringify(this.query))
      let result = ''
      Object.entries(_query).forEach(item => {
        result += `&${item[0]}=${item[1]}`
      })
      result = result.substr(1, result.length)

      return 'xxx地址' + '?' + result
```

## 除去json中的重复项

```js
 this.$api.origin.list().then(({ data: res }) => {
        // res.data.rows.forEach(item => {
        //   // this.activity.push({ value: item.activity_name, label: item.activity_name })
        // })
        var hash = {}
        // const newArr = []
        res.data.rows.forEach(item => {
          if (item.activity_name && !hash[item.activity_name]) {
            hash[item.activity_name] = true
            this.activity.push({ value: item.activity_name, label: item.activity_name })
          }
        })
      })
```

## el-dialog 组件

组件页

```vue
<template>
  <div>
    <el-dialog
      title="下载列表"
      :visible.sync="visible"
      :show="show"
      @close="$emit('update:show', false)"
    >
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      visible: this.show,
      formLabelWidth: '70px',
    }
  },
  watch: {
    show() {
      this.visible = this.show
    }
  }
}
</script>
```

引用页面

```vue
<template>
<el-button type="primary" class="vertical-middle " @click="show = true">下载</el-button>
<service-dialog :show.sync="show" />
</template>

<script>
import serviceDialog from './components/down'
export default {
  components: {
  serviceDialog
  },
  data() {
    return {
      show: false,
    }
  }
}
</script>
```

