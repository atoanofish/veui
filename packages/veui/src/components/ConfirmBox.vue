<template>
  <veui-dialog :open.sync="localOpen"
    :priority="priority"
    :closable="false"
    ui="reverse"
    overlay-class="veui-confirm-box">
    <template slot="title">
      <slot name="title">{{ title }}</slot>
    </template>
    <slot></slot>
    <template slot="foot">
      <veui-button ui="primary" @click="ok()">确定</veui-button>
      <veui-button ui="aux" @click="cancel()">取消</veui-button>
    </template>
  </veui-dialog>
</template>

<script>
import { pick } from 'lodash'
import Dialog from './Dialog'
import Button from './Button'
import config from '../managers/config'

config.defaults({
  'confirmbox.priority': 100
})

export default {
  name: 'veui-confirm-box',
  components: {
    'veui-dialog': Dialog,
    'veui-button': Button
  },
  props: {
    ...pick(Dialog.props, ['open', 'title']),
    ui: {
      type: String,
      default: 'reverse'
    }
  },
  data () {
    return {
      localOpen: this.open,
      localTitle: this.title,
      priority: config.get('confirmbox.priority')
    }
  },
  watch: {
    open (v) {
      this.localOpen = v
    },
    localOpen (v) {
      this.$emit('update:open', v)
    }
  },
  methods: {
    ok () {
      this.$emit('ok')
    },
    cancel () {
      this.$emit('cancel')
    }
  }
}
</script>
