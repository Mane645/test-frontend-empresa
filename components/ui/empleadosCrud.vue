<template>
  <div class="pa-3 ma-3">
    <v-row align="center" justify="center">
      <h1>Administracion de empleados</h1>
    </v-row>
    <v-row align="center" justify="end">
      <v-btn @click="dialogCreate = true">
        <v-icon>mdi-account</v-icon>
        <span style="text-transform: none;">
          Agregar nuevo
        </span>
      </v-btn>
    </v-row>
    <v-row align="center" justify="center">
      <v-data-table
        :headers="headers"
        :items="empleados"
      >
        <template #[`item.acciones`]="{ item }">
          <v-tooltip bottom color="green">
            <template #activator="{ on, attrs }">
              <v-btn
                icon
                color="warning"
                v-bind="attrs"
                @click="update(item)"
                v-on="on"
              >
                <v-icon>mdi-update</v-icon>
              </v-btn>
            </template>
            <span>Actualiza al empleado: {{ item.nombre }}</span>
          </v-tooltip>
          |
          <v-tooltip bottom color="red">
            <template #activator="{ on, attrs }">
              <v-btn
                icon
                color="red"
                v-bind="attrs"
                @click="deleteEmpleado(item.id)"
                v-on="on"
              >
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </template>
            <span>Borra al empleado: {{ item.nombre }}</span>
          </v-tooltip>
        </template>
      </v-data-table>
    </v-row>
    <v-dialog
      v-model="dialogBorrar"
      width="400"
      persistent
    >
      <v-card>
        <v-card-title>
          <v-row align="center" justify="center" class="pa-2 ma-2">
            Borrar empleado
          </v-row>
        </v-card-title>
        <v-card-text>
          <v-row align="center" justify="center" class="pa-2 ma-2">
            Estas seguro que quieres borrar el empleado
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-row align="center" justify="center" class="pa-2 ma-2">
            <v-btn color="warning" @click="dialogBorrar=false">
              <span style="text-transform: none;">
                Cancelar
              </span>
            </v-btn>
            |
            <v-btn color="red" @click="borralo">
              <span style="text-transform: none;">
                Borrar
              </span>
            </v-btn>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog
      v-model="dialogCreate"
      width="800"
      persistent
    >
      <signuo-form
        :inside="true"
        @click-cancel="dialogCreate = false"
        @guardado="empleadoGuardado"
      />
    </v-dialog>
    <v-dialog
      v-model="dialogUpdate"
      width="800"
      persistent
    >
      <signuo-form
        :inside="true"
        :update="true"
        :empleado-update="empleadoActualizar"
        @click-cancel="dialogUpdate = false"
        @guardado="empleadoGuardado"
      />
    </v-dialog>
  </div>
</template>

<script>
import signuoForm from './signuoForm.vue'
export default {
  components: {
    signuoForm
  },
  data () {
    return {
      empleados: [],
      headers: [
        {
          text: 'Nombre',
          align: 'center',
          sortable: true,
          value: 'nombre'
        },
        {
          text: 'A. Paterno',
          align: 'center',
          sortable: true,
          value: 'apaterno'
        },
        {
          text: 'A. Materno',
          align: 'center',
          sortable: true,
          value: 'amaterno'
        },
        {
          text: 'Direccion',
          align: 'center',
          sortable: true,
          value: 'direccion'
        },
        {
          text: 'Ciudad',
          align: 'center',
          sortable: true,
          value: 'ciudad'
        },
        {
          text: 'Estado',
          align: 'center',
          sortable: true,
          value: 'estado'
        },
        {
          text: 'Telefono',
          align: 'center',
          sortable: true,
          value: 'telefono'
        },
        {
          text: 'Acciones',
          align: 'center',
          sortable: true,
          value: 'acciones'
        }
      ],
      idBorrar: null,
      dialogBorrar: false,
      dialogCreate: false,
      dialogUpdate: false,
      empleadoActualizar: {}
    }
  },
  mounted () {
    this.getEmpleados()
  },
  methods: {
    async getEmpleados () {
      const res = await this.$axios.get('/empleados')
      console.log('@@@ res => ', res)
      if (res && res.data && res.data.success) {
        this.empleados = []
        this.empleados = res.data.empleados
      }
    },
    deleteEmpleado (id) {
      this.idBorrar = id
      this.dialogBorrar = true
    },
    async borralo () {
      const borrado = await this.$axios.delete(`/empleados/delete/${this.idBorrar}`)
      if (borrado && borrado.data && borrado.data.success) {
        this.$store.commit('setType', 'error')
        this.$store.commit('setColor', 'red')
        this.$store.commit('setMensaje', 'El usuario fue borrado exitosamente')
        this.$store.commit('setShowAlert', true)
        await this.getEmpleados()
      }
      console.log('@@@ borrado => ', borrado)
      this.dialogBorrar = false
    },
    empleadoGuardado () {
      this.getEmpleados()
      this.dialogCreate = false
      this.dialogUpdate = false
    },
    update (item) {
      this.empleadoActualizar = item
      console.log('@@@ empleado => ', item, this.empleadoActualizar)
      this.dialogUpdate = true
    }
  }
}
</script>

<style scoped>
</style>
