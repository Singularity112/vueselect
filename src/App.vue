<template>
  <div id="app">
    <div class="container">
      <div class="col">
        selected: {{ select1Model }}
        <VueSelect :items="select1" v-model="select1Model"/>
      </div>
      <div class="col">
        selected: {{ select2Model }}
        <VueSelect :items="select2" v-model="select2Model" />
      </div>
    </div>
  </div>
</template>

<script>
import VueSelect from './components/VueSelect.vue'
import arr from '@/components/arr'

export default {
  name: 'App',
  components: {
    VueSelect
  },
  data () {
    return {
      select1Model: '',
      select1: arr,
      select2Model: '',
      select2: [
        'item1',
        'item2',
        'item3',
        'item4',
        'item5',
        'item6',
        'item7',
        'item8',
        'item9',
        'item10'
      ]
    }
  }
}
</script>
<style scoped lang="scss">
  .container {
    display: flex;
    flex-direction: row;

    .col {
      flex: 1;
      padding-left: 15px;
      padding-right: 15px;
      box-sizing: border-box;
    }
  }
</style>