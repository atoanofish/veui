<template>
  <span v-if="!to"><slot></slot></span>
  <router-link v-else-if="$router && !native"
    :to="to"
    :replace="replace">
    <slot></slot>
  </router-link>
  <a v-else
    :href="to"
    @click="handleRedirect">
    <slot></slot>
  </a>
</template>
<script>
export default {
  name: 'veui-link',
  props: {
    to: {
      type: String,
      default: ''
    },
    replace: {
      type: Boolean,
      default: false
    },
    native: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    handleRedirect (event) {
      if (this.to) {
        this.$emit('click', event)

        if (this.replace && !event.defaultPrevented) {
          event.preventDefault()
          location.replace(this.to)
        }
      } else {
        event.preventDefault()
        this.$emit('click', event)
      }
    }
  }
}
</script>
