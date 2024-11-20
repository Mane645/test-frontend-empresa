<template>
  <v-app>
    <show-alert
      v-if="showAlert"
      :type="type"
      :color="color"
      :mensaje="mensaje"
      class="show-alert"
    />
    <v-main>
      <Nuxt />
    </v-main>
    <v-navigation-drawer
      permanent
      absolute
      right
      app
    >
      <template #prepend>
        <v-list-item two-line>
          <v-list-item-avatar>
            <img src="https://randomuser.me/api/portraits/men/23.jpg">
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>Jane Smith</v-list-item-title>
            <v-list-item-subtitle>Logged In</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </template>

      <v-divider />

      <v-list dense>
        <v-list-item
          v-for="item in items"
          :key="item.title"
          link
          :to="item.path"
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
import { mapState } from 'vuex'
export default {
  data () {
    return {
      items: [
        {
          title: 'Empleados',
          icon: 'mdi-account-group',
          path: '/dashboard/empleados'
        }
      ]
    }
  },
  // leyendo variable del store
  computed: {
    ...mapState({
      showAlert: state => state.showAlert,
      type: state => state.type,
      color: state => state.color,
      mensaje: state => state.mensaje
    })
  },
  watch: {
    showAlert () {
      console.log('@@@ cambio =>', this.showAlert)
      if (this.showAlert) {
        setTimeout(() => {
          this.$store.commit('setShowAlert', false)
        })
      }
    }
  },
  type () {},
  color () {},
  mensaje () {}
}
</script>

<style scoped>
.show-alert {
  position: fixed;
  left: 10px;
  z-index: 100;
  top: 10px;
  min-width: 350px;
}
</style>
