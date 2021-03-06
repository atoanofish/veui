<template>
<table class="veui-table" :ui="ui">
  <colgroup>
    <col v-if="selectable" width="60"></col>
    <col v-for="(col, index) in realColumns" :width="col.width" :key="index"></col>
  </colgroup>
  <table-head @sort="sort"></table-head>
  <table-body><template slot="no-data"><slot name="no-data">没有数据</slot></template></table-body>
  <slot name="foot"><table-foot v-if="hasFoot"></table-foot></slot>
  <slot></slot>
</table>
</template>

<script>
import { map, intersection, isString, includes, indexOf, keys as objectKeys } from 'lodash'
import Body from './_Body'
import Head from './_Head'
import Foot from './_Foot'

export default {
  name: 'veui-table',
  uiTypes: ['table'],
  components: {
    'table-body': Body,
    'table-head': Head,
    'table-foot': Foot
  },
  props: {
    ui: String,
    data: {
      type: Array,
      default () {
        return []
      }
    },
    keys: {
      validator (value) {
        if (!value) {
          return true
        }
        return isString(value) || Array.isArray(value) && value.length === this.data.length
      }
    },
    selectable: Boolean,
    order: [String, Boolean],
    orderBy: String,
    columnFilter: Array,
    selected: Array
  },
  data () {
    return {
      columns: [],
      localSelectedKeys: [...(this.selected || [])]
    }
  },
  computed: {
    realColumns () {
      if (!this.columnFilter) {
        return this.columns
      }
      return this.columns.filter(col => includes(this.columnFilter, col.field))
    },
    realKeys () {
      let keys = this.keys
      if (!keys) {
        keys = objectKeys(this.data)
      }
      if (typeof keys === 'string') {
        keys = map(this.data, keys)
      }
      return keys.map(String)
    },
    selectedItems () {
      return (this.localSelectedKeys || []).reduce((selectedItems, key) => {
        selectedItems[key] = this.getItem(key)
        return selectedItems
      }, {})
    },
    selectStatus () {
      let keys = this.realKeys
      let inter = intersection(keys, this.localSelectedKeys)
      if (!inter.length) {
        return 'none'
      }
      if (inter.length === keys.length) {
        return 'all'
      }
      return 'partial'
    },
    hasFoot () {
      return this.$slots.foot || this.columns.some(col => col.hasFoot)
    }
  },
  methods: {
    select (selected, index) {
      if (index !== undefined) {
        let item = this.data[index]
        let key = this.realKeys[index]
        if (selected) {
          this.localSelectedKeys.push(key)
        } else {
          this.localSelectedKeys.splice(indexOf(this.localSelectedKeys, key), 1)
        }
        this.$emit('select', selected, item, this.selectedItems)
      } else {
        if (selected) {
          this.localSelectedKeys = [...this.realKeys]
        } else {
          this.localSelectedKeys = []
        }
        this.$emit('select', selected, this.selectedItems)
      }
      this.$emit('update:selected', this.localSelectedKeys)
    },
    getItem (key) {
      return this.data[indexOf(this.realKeys, key)]
    },
    sort (field, order) {
      this.$emit('sort', field, order)
    }
  },
  watch: {
    selected (value) {
      this.localSelectedKeys = value
    },
    realKeys (value) {
      this.localSelectedKeys = intersection(this.localSelectedKeys, value)
      this.$emit('update:selected', this.localSelectedKeys)
    }
  }
}
</script>
