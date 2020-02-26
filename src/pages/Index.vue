<template>
  <q-page class="">
    <h1 class="text-h4 text-center text-primary">Autocomplete example</h1>
    <div class="row justify-center">
      <autocomplete class="q-pa-lg col-6" :list="results" label="aeaee" mainKey="name" :searchKeys="['age', 'name']" :inputProps="{ placeholder: 'Search for name, age...', filled: true }" />
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
          name: 'John',
          age: '24'
        },
        {
          name: 'Roberta',
          age: '23'
        },
        {
          name: 'carlos',
          age: '22'
        },
        {
          name: 'Douglas',
          age: '22'
        }
      ]
    }
  }
}
</script>
