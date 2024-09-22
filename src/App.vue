<script setup>
import { watch, defineComponent, reactive } from 'vue'
import { useQuery } from '@vue/apollo-composable'
import gql from 'graphql-tag'
import { use } from 'echarts/core';
import { LineChart } from 'echarts/charts';
import { GridComponent, TooltipComponent, TitleComponent, LegendComponent } from 'echarts/components';
import { CanvasRenderer } from 'echarts/renderers';
import VChart from 'vue-echarts';


// zapytania mozna trzymac w innych modulach
// przerobic zapytania aby byly ze zmiennymi i filtracja po datach
const GET_DATA = gql`
query {
  hdfData {
    x
    y
  }
}
`

const { result } = useQuery(GET_DATA)

// potrzebne komponenty ECharts
use([CanvasRenderer, LineChart, GridComponent, TooltipComponent, TitleComponent, LegendComponent])


let chartOptions = reactive({
  title: {
    text: 'Wykres'
  },
  tooltip: {
    trigger: 'axis'
  },
  xAxis: {
    type: 'category',
    data: [], // tutaj trafiają dane z result.x
    name: 'X'
  },
  yAxis: {
    type: 'value',
    name: 'Y'
  },
  series: [
    {
      name: 'Dane Y',
      data: [], // tutaj trafiają dane z result.y
      type: 'line',
      smooth: true
    }
  ]
});

// Aktualizacja opcji wykresu, gdy dane są załadowane zmienic to i tak zeby plynnie leciał wykres
watch(result, (newResult) => {
  console.log(newResult)
  if (newResult) {
    chartOptions.xAxis.data = newResult.hdfData.x; // dane dla osi X
    chartOptions.series[0].data = newResult.hdfData.y; // dane dla osi Y
  }
})
</script>

<template>
   <v-chart :option="chartOptions" style="width: 600px; height: 400px;" />
</template>

