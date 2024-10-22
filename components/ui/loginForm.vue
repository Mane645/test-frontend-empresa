<template>
  <v-card flat elevation="0" width="400" color="#E5D9F2">
    <v-card-title>
      <p class="text-center" color="#A594F9" style="width: 100% !important;">
        Welcome to my business
      </p>
    </v-card-title>
    <v-card-text>
      <v-row style="width: 100% !important;">
        <v-col cols="4">
          <v-img :src="require('@/assets/images/anya.jpg')" />
        </v-col>
        <v-col cols="8">
          <form>
            <v-row>
              <v-text-field
                v-model="form.usuario"
                outlined
                dense
                label="Usuario"
              />
            </v-row>
            <v-row>
              <v-text-field
                v-model="form.password"
                type="password"
                outlined
                dense
                label="Password"
              />
            </v-row>
            <v-row v-if="errorMessage" class="error">
              {{ errorMessage }}
            </v-row>
          </form>
        </v-col>
      </v-row>
    </v-card-text>
    <v-card-actions>
      <v-row align="center" justify="center" class="ma-3">
        <v-btn color="#CDC1FF" @click="gotoSignUp">
          <span style="text-transform: none; color: #F5EFFF;">
            Registrarse
          </span>
        </v-btn>
        <v-btn color="#6C48C5" class="ml-4" @click="login">
          <span style="text-transform: none; color: #F5EFFF;">
            Iniciar sesion
          </span>
        </v-btn>
      </v-row>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      form: {
        usuario: '',
        password: ''
      },
      errorMessage: ''
    }
  },
  methods: {
    async login () {
      try {
        const res = await this.$auth.loginWith('local', {
          data: this.form
        })
        console.log('@@@ res => ', res)
      } catch (error) {
        this.errorMessage = error
      }
    },
    gotoSignUp () {
      this.$router.push('/signup')
    }
  }
}
</script>

<style scoped>
.error {
  color: red;
}
</style>
