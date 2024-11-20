<template>
  <empleados-crud />
</template>

<script>
import empleadosCrud from '~/components/ui/empleadosCrud.vue'
export default {
  components: {
    empleadosCrud
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>
</style>
