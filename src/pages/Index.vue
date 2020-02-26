<template>
  <q-page class="">
    <h1 class="text-h4 text-center text-primary">Autocomplete example</h1>
    <div class="row justify-center">
      <autocomplete class="q-pa-lg col-6" :list="results" filled :searchKeys="['name', 'age', 'test']" />
    </div>
  </q-page>
</template>

<script>
import Autocomplete from '../components/Autocomplete'

export default {
  name: 'PageIndex',

  components: {
    Autocomplete
  },

  data () {
    return {
      results: [
        {
          name: 'douglas',
          age: '24'
        },
        {
          name: 'joão',
          age: '23'
        },
        {
          name: 'carlos',
          age: '22'
        },
        {
          name: 'douglas calora',
          age: '22',
          test: 'hue'
        }
      ]
    }
  }
}
</script>
