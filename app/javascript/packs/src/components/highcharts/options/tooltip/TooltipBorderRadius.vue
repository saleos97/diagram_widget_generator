<template>

  <div class="mb-4 row">
    <div class="col-4 text-right">
      <label class="form-label mt-2" for="border_radius_tooltip">Border Radius</label>
    </div>
    <div class="col-8">
      <input
              id="border_radius_tooltip"
              type="number"
              v-model="tooltipBorderRadius"
              class="form-control"
      />
    </div>
  </div>

</template>

<script>
  import { computed, ref, watch } from "vue";
  import { useStore } from 'vuex';
  export default {
    setup() {
      const store = useStore();
      const hasTooltipBorderRadius = computed(function () {
        return store.getters.highChartsOptions.tooltip?.borderRadius ?
          store.getters.highChartsOptions.tooltip?.borderRadius :
          3
      })
      const tooltipBorderRadius = ref(hasTooltipBorderRadius.value);
      const highChartsOptions = computed(function () {
        return store.getters.highChartsOptions
      })

      if(!highChartsOptions.value.tooltip){
        store.dispatch('changePropertyWithOneKey', {
          first_key: 'tooltip',
          data: {}
        })
      }

      if(!highChartsOptions.value.tooltip.borderRadius){
        store.dispatch('changePropertyWithTwoKeys', {
          first_key: 'tooltip',
          second_key: 'borderRadius',
          data: 3
        })
      }

      watch(tooltipBorderRadius, function (newValue) {
        store.dispatch('changePropertyWithTwoKeys', {
          first_key: 'tooltip',
          second_key: 'borderRadius',
          data: newValue
        })
      })

      return { tooltipBorderRadius }
    }
  }

</script>
