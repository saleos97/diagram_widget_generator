<template>
  <div class="mb-4 row">
    <div class="col-4 text-right">
      <label for="reverse_xAxis" >Werte inversieren</label>
    </div>
    <div class="col-8">
      <input id="reverse_xAxis" v-model="reversed" type="checkbox"/>
    </div>
  </div>
</template>
<script>
  import { useStore } from 'vuex';
  import { computed,ref,watch } from "vue";
  export default {
    setup(){
      const store = useStore();
      const hasReversed = computed(function () {
        return store.getters.highChartsOptions.xAxis?.reversed ?
          store.getters.highChartsOptions.xAxis.reversed : false;
      })


      const reversed = ref(hasReversed.value)
      const highChartsOptions = computed(function () { return store.getters.highChartsOptions })

      if(!highChartsOptions.value.xAxis){
        store.dispatch('changePropertyWithOneKey', {
          first_key: 'xAxis',
          data: {}
        })
      }
      if(!highChartsOptions.value.xAxis.reversed){
        store.dispatch('changePropertyWithTwoKeys', {
          first_key: 'xAxis',
          second_key: 'reversed',
          data: false
        })
      }

      watch(reversed, function (newValue) {
        store.dispatch('changePropertyWithTwoKeys', {
          first_key: 'xAxis',
          second_key: 'reversed',
          data: newValue
        })
      })


      return { reversed }
    }
  }
</script>