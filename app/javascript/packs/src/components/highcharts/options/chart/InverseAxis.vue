<template>
  <div class="mb-4 row">
    <div class="col-4 text-right">
      <label for="inverse_axis">Achsen umkehren</label>
    </div>
    <div class="col-8">
      <input id="inverse_axis" v-model="inverseAxis" type="checkbox" class=""/>
    </div>
  </div>
</template>
<script>
  import {computed, ref, watch} from 'vue';
  import { useStore } from 'vuex';
  export default {
    setup() {
      const store = useStore();
      const hasInverseAxis = computed(function () {
        return store.getters.highChartsOptions.chart?.inverted ? store.getters.highChartsOptions.chart?.inverted : false;
      })
      const inverseAxis = ref(hasInverseAxis.value);
      const highChartsOptions = computed(function () {
        return store.getters.highChartsOptions
      })

      if(!highChartsOptions.value.chart) {
        store.dispatch('changePropertyWithOneKey', {
          first_key: 'chart',
          data: {}
        })
      }
      if(!highChartsOptions.value.chart.inverted) {
        store.dispatch('changePropertyWithTwoKeys', {
          first_key: 'chart',
          second_key: 'inverted',
          data: false
        })
      }

      watch(inverseAxis, function (newValue) {
        store.dispatch('changePropertyWithTwoKeys', {
          first_key: 'chart',
          second_key: 'inverted',
          data: newValue
        })
      })
      return { inverseAxis }
    }
  }
</script>