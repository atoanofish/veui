<template>
  <article>
    <h1><code>&lt;veui-checkbox&gt;</code></h1>
    <p>
      <veui-checkbox value="normal" v-model="picked1">正常状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox value="small" ui="small" v-model="picked2">正常状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox v-model="picked3">选中状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox ui="small" v-model="picked4">选中状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox indeterminate v-model="picked5">部分选中状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox ui="small" indeterminate v-model="picked6">部分选中状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox disabled v-model="picked7">选中无效状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox ui="small" disabled v-model="picked8">选中无效状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox disabled v-model="picked9">未选无效状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox ui="small" disabled v-model="picked10">未选无效状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox disabled indeterminate v-model="picked11">部分选中无效状态</veui-checkbox>
    </p>
    <p>
      <veui-checkbox ui="small" disabled indeterminate v-model="picked12">部分选中无效状态</veui-checkbox>
    </p>
  </article>
</template>

<script>
import { Checkbox } from 'veui'

export default {
  name: 'checkbox-demo',
  components: {
    'veui-checkbox': Checkbox
  },
  data () {
    return {
      picked1: false,
      picked2: false,
      picked3: true,
      picked4: true,
      picked5: false,
      picked6: false,
      picked7: true,
      picked8: true,
      picked9: false,
      picked10: false,
      picked11: false,
      picked12: false
    }
  }
}
</script>
