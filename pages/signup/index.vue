<template>
  <signup-form />
</template>

<script>
import signupForm from '~/components/ui/signuoForm.vue'
export default {
  components: {
    signupForm
  },
  auth: false,
  layout: 'login'
}
</script>

<style scoped>
</style>
