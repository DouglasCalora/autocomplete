<template>
  <div v-bind="$attrs" v-on="$listeners">
    <q-input v-bind="$attrs" v-model="search" :debounce="debounce" :loading="loadingState" @input.native="setLoading">
      {{search}}
      <template v-slot:append>
        <q-btn v-if="hasResult" icon="close" flat @click="clearSeach"/>
        <q-btn icon="search" flat/>
      </template>
    </q-input>
    <slot :result="result">
      <transition name="fade">
        <div v-if="hasResult" class="shadow-1">
          <q-list dense bordered padding class="rounded-borders">
            <q-item v-for="(item, index) in result" :key="index" clickable v-ripple>
              <q-item-section>
                <div @click="setSearch(item[mainKey])" v-html="formattedResult[index][mainKey]"/>
              </q-item-section>
            </q-item>
        </q-list>
        </div>
      </transition>
    </slot>
  </div>
</template>

<script>
export default {
  props: {
    list: {
      type: Array,
      default: () => []
    },

    searchKeys: {
      type: [String, Array],
      default: 'name'
    },

    mainKey: {
      type: String,
      default: 'name'
    },

    debounce: {
      type: String,
      default: '500'
    }
  },

  data () {
    return {
      result: [],
      formattedResult: [],
      search: '',
      loadingState: false
    }
  },

  watch: {
    search (value) {
      this.filter(value)
    },

    loadingState (value) {
      if (value && !this.search) {
        setTimeout(() => {
          this.loadingState = false
        }, 1000)
      }
    }
  },

  computed: {
    hasResult () {
      return this.result.length
    }
  },

  methods: {
    filter (value) {
      console.log('fui chamado')
      this.clearResults()

      if (!value) {
        return null
      }

      value = value.trim().replace(/\s{2,}/g, ' ').toLowerCase()

      this.list.forEach((result, listIndex) => {
        if (Array.isArray(this.searchKeys)) {
          this.searchKeys.forEach(key => {
            if (result[key] && result[key] && result[key].toLowerCase().search(value) >= 0) {
              this.increment(result, value)
            }
          })
        } else if (result[this.searchKeys] && result[this.searchKeys].toLowerCase().search(value) >= 0) {
          this.increment(result, value)
        }
      })

      this.loadingState = false
    },

    increment (result, value) {
      this.result.push(result)
      this.formattedResult.push(result)
      this.insertSpan(result, value)
    },

    setLoading () {
      this.loadingState = !this.hasResult
    },

    insertSpan (result, value) {
      this.$set(
        this.formattedResult,
        this.formattedResult.length - 1,
        {
          ...this.formattedResult[this.formattedResult.length - 1],
          [this.mainKey]: result[this.mainKey].replace(value, `<span class="hightlight">${value}</span>`)
        }
      )
    },

    setSearch (value) {
      this.search = value
    },

    clearSeach () {
      this.search = ''
    },

    clearResults () {
      this.result = []
      this.formattedResult = []
    }
  }
}
</script>

<style lang="scss">
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s, height 1s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
  height: 0;
}

.hightlight {
  color: $primary;
}
</style>
