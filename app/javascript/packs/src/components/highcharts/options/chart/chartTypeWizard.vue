<template>
  <div v-for="type in chartConfiguration.type"
       :id="'chart_'+type.value"
       :key="type.value"
       class="card my-1 pointer"
       @click="changeBackground(type.value)" >
    <div class="row">
      <div class="col-sm-2 border-right">
        <i :class="chartClass(type)"></i>
      </div>
      <div class="col-sm-10 my-auto">
        <p class="text-left" style="font-size: 23px">{{type.text}}</p>
      </div>
    </div>
  </div>


</template>

<script>
  import { ref, onMounted } from "vue";
  import { useStore } from 'vuex';
  import arrayToCsvConverter from "../../../../mixins/arrayToCsv";

  export default {
    emits: ['isClicked'],
    setup(props, { emit }){
      const store = useStore()
      const chartConfiguration = store.getters.chartConfiguration;
      let lastClicked;
      const { headerToCsv, arrayToCsv } = arrayToCsvConverter()


      const chart = ref(store.getters.highChartsOptions.chart?.type)

      const chartClass = function (item) {
        return `m-1 fas fa-chart-${item.fontawesome} fa-4x`
      }


      function changeBackground(chartType) {
        const element = document.getElementById(`chart_${chartType}`);
        element.classList.add('bg-light');
        if(lastClicked && lastClicked !== element){
          lastClicked.classList.remove('bg-light');
        }
        updateChartType(chartType);
        updateChartData();
        emit('isClicked', true);
        lastClicked = element;
      }


      function updateChartType(chartType) {
        store.dispatch('changePropertyWithTwoKeys', {
          first_key: 'chart',
          second_key: 'type',
          data: chartType
        })
      }

      function updateChartData() {
        if(!store.getters.highChartsOptions.data?.csv){
          const exampleData = store.getters.exampleData;
          store.dispatch('changePropertyWithOneKey', {
            first_key: 'series',
            data: exampleData.series
          })
          store.dispatch('changePropertyWithOneKey', {
            first_key: 'data',
            data: {
              csv: exampleData.csv
            }
          })
        }
      }
      



      if(!store.getters.highChartsOptions.chart){
        store.dispatch('changePropertyWithOneKey', {
          first_key: 'chart',
          data: {}
        })
      }

      onMounted(function () {
        if(chart.value){
          const element = document.getElementById(`chart_${chart.value}`);
          element.classList.add('bg-light');
          lastClicked = element
        }
      })

      return {chartConfiguration, chartClass, lastClicked, changeBackground }
    }

  }
</script>


<style scoped>
  .pointer {
    cursor: pointer;
  }

</style>