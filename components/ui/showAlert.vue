<template>
  <v-alert
    :type="type"
    :color="color"
    shaped
  >
    <span>
      {{ mensaje }}
    </span>
  </v-alert>
</template>

<script>
export default {
  props: {
    type: { type: String, default: 'success' },
    color: { type: String, default: 'green' },
    mensaje: { type: String, default: '' }
  },
  data () {
    return {
    }
  }
}
</script>

<style scoped>
</style>
