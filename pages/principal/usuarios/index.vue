<template>
  <v-col cols="12">
    <v-row>
      <v-btn block color="green" @click="showNuevo = true">
        <span style="color: white; text-transform: none;">
          Usuario Nuevo
        </span>
      </v-btn>
    </v-row>
    <v-row class="mt-3">
      <v-data-table
        :headers="headers"
        :items="usuarios"
        elevation="0"
        style="width: 100%!important;"
      >
        <template #[`item.acciones`]="{item}">
          <v-row>
            <v-col cols="6">
              <v-btn icon color="red" @click="borrarUsuario(item.id)">
                <v-icon>
                  mdi-account-minus
                </v-icon>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn icon color="warning" @click="actualizarUsuario(item)">
                <v-icon>
                  mdi-account-eye
                </v-icon>
              </v-btn>
            </v-col>
          </v-row>
        </template>
      </v-data-table>
    </v-row>
    <v-dialog v-model="showDelete" width="400" persistent>
      <v-card>
        <v-card-title>Usuarios</v-card-title>
        <v-card-text>Are You Sure?</v-card-text>
        <v-card-acctions>
          <v-row>
            <v-col cols="6">
              <v-btn block color="red" @click="borrar">
                <span style="color: white; text-transform: none;">
                  Borrar
                </span>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn block color="green" @click="showDelete = false">
                <span style="color: white; text-transform: none;">
                  Cancelar
                </span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-acctions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="showNuevo" width="400" persistent>
      <v-card>
        <v-card-title>Registrar Usuario</v-card-title>
        <v-card-text>
          <v-form
            ref="form"
            v-model="validForm"
          >
            Nombre:
            <v-text-field
              v-model="nombre"
              placeholder="Escribe tu nombre"
              type="nombre"
            />
            Apaterno:
            <v-text-field
              v-model="apaterno"
              placeholder="Escribe tu Apaterno"
              type="apaterno"
            />
            Amaterno:
            <v-text-field
              v-model="amaterno"
              placeholder="Escribe tu Amaterno"
              type="amaterno"
            />
            Direccion:
            <v-text-field
              v-model="direccion"
              placeholder="Escribe tu direccion"
              type="direccion"
            />
            Telefono:
            <v-text-field
              v-model="telefono"
              placeholder="Escribe tu telefono"
              type="telefono"
            />
            Estado:
            <v-select
              v-model="estado"
              :items="estadoss"
              placeholder="Selecciona tu estado"
            />
            Email:
            <v-text-field
              v-model="email"
              placeholder="Escribe tu correo"
              type="email"
              :rules="correo"
            />
            Password:
            <v-text-field
              v-model="passwordUser"
              placeholder="Escribe tu password"
              type="password"
              :rules="password"
            />
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-row>
            <v-col cols="6">
              <v-btn block color="primary" @click="agregar">
                <span style="color: aliceblue; text-transform: none;">
                  Agregar
                </span>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn block color="red" @click="showNuevo = false">
                <span style="color: white; text-transform: none;">
                  Cancelar
                </span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="showUpdate" width="400" persistent>
      <v-card>
        <v-card-title>Modificar Usuario</v-card-title>
        <v-card-text>
          <v-form
            ref="formUpdate"
            v-model="validFormUpdate"
          >
            Nombre:
            <v-text-field
              v-model="userToUpdate.nombre"
              placeholder="Escribe tu nombre"
              type="nombre"
            />
            Apaterno:
            <v-text-field
              v-model="userToUpdate.apaterno"
              placeholder="Escribe tu Apaterno"
              type="apaterno"
            />
            Amaterno:
            <v-text-field
              v-model="userToUpdate.amaterno"
              placeholder="Escribe tu Amaterno"
              type="amaterno"
            />
            Direccion:
            <v-text-field
              v-model="userToUpdate.direccion"
              placeholder="Escribe tu direccion"
              type="direccion"
            />
            Telefono:
            <v-text-field
              v-model="userToUpdate.telefono"
              placeholder="Escribe tu telefono"
              type="telefono"
            />
            Estado:
            <v-select
              v-model="userToUpdate.estado"
              :items="estadoss"
              placeholder="Selecciona tu estado"
            />
            Email:
            <v-text-field
              v-model="userToUpdate.email"
              placeholder="Escribe tu correo"
              type="email"
              :rules="correo"
            />
            Password:
            <v-text-field
              v-model="userToUpdate.password"
              placeholder="Escribe tu password"
              type="password"
              :rules="password"
            />
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-row>
            <v-col cols="6">
              <v-btn block color="primary" @click="modificar">
                <span style="color: aliceblue; text-transform: none;">
                  Modificar
                </span>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn block color="red" @click="showUpdate = false">
                <span style="color: white; text-transform: none;">
                  Cancelar
                </span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-col>
</template>

<script>
export default {
  layout: 'default',
  data () {
    return {
      headers: [
        {
          text: 'id',
          align: 'center',
          sortable: true,
          value: 'id'
        },
        {
          text: 'nombre', // agregamos nombre y eso
          aling: 'center',
          sortable: true,
          value: 'nombre'
        },
        {
          text: 'apaterno',
          aling: 'center',
          sortable: true,
          value: 'apaterno'
        },
        {
          text: 'amaterno',
          aling: 'center',
          sortable: true,
          value: 'amaterno'
        },
        {
          text: 'direccion',
          aling: 'center',
          sortable: true,
          value: 'direccion'
        },
        {
          text: 'telefono',
          aling: 'center',
          sortable: true,
          value: 'telefono'
        },
        {
          text: 'estado',
          aling: 'center',
          sortable: true,
          value: 'estado'
        },
        {
          text: 'email',
          align: 'center',
          sortable: true,
          value: 'email'
        },
        {
          text: 'acciones crud',
          align: 'center',
          sortable: false,
          value: 'acciones'
        }
      ],
      token: null,
      usuarios: [],
      showDelete: false,
      idToDelete: null,
      showNuevo: false,
      validForm: false,
      nombre: null, //
      apaterno: null,
      amaterno: null,
      direccion: null,
      telefono: null,
      estadoss: [
        'Aguascalientes',
        'Baja California',
        'Baja California Sur',
        'Campeche',
        'Chiapas',
        'Chihuahua',
        'Ciudad de México',
        'Coahuila',
        'Colima',
        'Durango',
        'Guanajuato',
        'Guerrero',
        'Hidalgo',
        'Jalisco',
        'México',
        'Michoacán',
        'Morelos',
        'Nayarit',
        'Nuevo León',
        'Oaxaca',
        'Puebla',
        'Querétaro',
        'Quintana Roo',
        'San Luis Potosí',
        'Sinaloa',
        'Sonora',
        'Tabasco',
        'Tamaulipas',
        'Tlaxcala',
        'Veracruz',
        'Yucatán',
        'Zacatecas'
      ],
      email: null,
      passwordUser: null,
      password: [
        v => (v && v.length > 6) || 'Password must be more than 6 chars'
      ],
      correo: [
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
      ],
      showUpdate: false,
      userToUpdate: {},
      validFormUpdate: false
    }
  },
  mounted () {
    this.token = localStorage.getItem('token')
    if (!this.token) {
      this.$router.push('/')
    }
    this.getAllUsers()
  },
  methods: {
    getAllUsers () {
      const url = '/get-allusers'
      const config = {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }
      this.$axios.get(url, config)
        .then((res) => {
          if (res.data.message === 'Success') {
            this.usuarios = res.data.users
          } else if (res.data.message === 'Invalid Token') {
            this.$router.push('/')
          }
        })
        .catch((err) => {
          this.$router.push('/')
          // eslint-disable-next-line no-console
          console.log('@@@ err => ', err)
        })
    },
    borrarUsuario (id) {
      this.idToDelete = id
      this.showDelete = true
    },
    borrar () {
      const url = `/delete-user/${this.idToDelete}`
      const config = {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }
      this.$axios.delete(url, config)
        .then((res) => {
          this.getAllUsers()
          this.showDelete = false
          if (res.data.message === 'User deleted succesfully') {
            this.$nuxt.$emit('evento', {
              message: res.data.message,
              color: 'red',
              type: 'error'
            })
            this.getAllUsers()
            this.showDelete = false
          }
        })
        .catch((err) => {
        // eslint-disable-next-line no-console
          console.log('@@@ err => ', err)
        })
    },
    agregar () {
      this.validForm = this.$refs.form.validate()
      if (this.validForm) {
        const sendData = {
          id: Date.now().toString(),
          email: this.email,
          password: this.passwordUser,
          nombre: this.nombre, //
          apaterno: this.apaterno,
          amaterno: this.amaterno,
          direccion: this.direccion,
          telefono: this.telefono,
          estado: this.estado //
        }
        const url = '/signup'
        this.$axios.post(url, sendData)
          .then((res) => {
            if (res.data.message === 'Usuario Registrado Satisfactoriamente') {
              this.$nuxt.$on('evento', (data) => { //
                this.message = data.message
                this.color = data.color
                this.type = data.type
                this.showAlert = true
                setTimeout(() => {
                  this.showAlert = false
                }, 5000)
              })
              this.getAllUsers()
              this.showNuevo = false
            }
          })
          .catch((err) => {
            // eslint-disable-next-line no-console
            console.log('@@@ err => ', err)
          })
      } else {
        alert('Faltan Datos')
      }
    },
    actualizarUsuario (user) {
      this.userToUpdate = user
      this.showUpdate = true
    },
    modificar () {
      this.validFormUpdate = this.$refs.formUpdate.validate()
      if (this.validFormUpdate) {
        const sendData = {
          id: this.userToUpdate.id,
          email: this.userToUpdate.email,
          password: this.userToUpdate.password,
          nombre: this.userToUpdate.nombre, //
          apaterno: this.userToUpdate.apaterno,
          amaterno: this.userToUpdate.amaterno,
          direccion: this.userToUpdate.direccion,
          telefono: this.userToUpdate.telefono,
          estado: this.userToUpdate.estado //
        }
        this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token}`
        const url = `/update-user/${sendData.id}`
        this.$axios.put(url, sendData)
          .then((res) => {
            if (res.data.message === 'User Updated successfully') {
              this.$nuxt.$emit('evento', { //
                message: res.data.message,
                color: 'warning',
                type: 'success',
                time: 3000
              })
              this.getAllUsers()
              this.showUpdate = false
            }
          })
          .catch((err) => {
            // eslint-disable-next-line no-console
            console.log('@@@ err => ', err)
          })
      } else {
        alert('Faltan Datos')
      }
    }
  }
}
</script>
