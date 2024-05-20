<template>
  <v-card
    color="#C5CAE9"
    elevation="0"
    width="500"
  >
    <v-cart-title>
      LOGIN
    </v-cart-title>
    <v-card-text>
      <v-form
        ref="form"
        v-model="validForm"
      >
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
          <v-btn block color="green" elevation="0" @click="loginUser">
            <span style="text-transform: none; color: white;">
              LOGIN
            </span>
          </v-btn>
        </v-col>
        <v-col cols="6">
          <v-btn block color="red" elevation="0" @click="showNuevo = true">
            <span style="text-transform: none; color: white;">
              SIGNUP
            </span>
          </v-btn>
        </v-col>
      </v-row>
    </v-card-actions>
    <v-dialog v-model="showNuevo" width="400" persistent>
      <v-card>
        <v-card-title>Registrar Usuario</v-card-title>
        <v-card-text>
          <v-form
            ref="formNuevo"
            v-model="validFormNuevo"
          >
            Nombre:
            <v-text-field
              v-model="nombreNuevo"
              placeholder="Escribe tu nombre"
              type="nombre"
            />
            Apaterno:
            <v-text-field
              v-model="apaternoNuevo"
              placeholder="Escribe tu Apaterno"
              type="apaterno"
            />
            Amaterno:
            <v-text-field
              v-model="amaternoNuevo"
              placeholder="Escribe tu Amaterno"
              type="amaterno"
            />
            Direccion:
            <v-text-field
              v-model="direccionNuevo"
              placeholder="Escribe tu direccion"
              type="direccion"
            />
            Telefono:
            <v-text-field
              v-model="telefonoNuevo"
              placeholder="Escribe tu telefono"
              type="telefono"
            />
            Estado:
            <v-select
              v-model="estadoNuevo"
              :items="estadoss"
              placeholder="Selecciona tu estado"
            />
            Email:
            <v-text-field
              v-model="emailNuevo"
              placeholder="Escribe tu correo"
              type="email"
              :rules="correo"
            />
            Password:
            <v-text-field
              v-model="passwordUserNuevo"
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
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      validForm: false,
      email: null,
      passwordUser: null,
      // Para usuario nuevo
      validFormNuevo: false,
      emailNuevo: null,
      passwordUserNuevo: null,
      nombreNuevo: null, //
      apaternoNuevo: null,
      amaternoNuevo: null,
      direccionNuevo: null,
      telefonoNuevo: null,
      estadoNuevo: null, //
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
      showNuevo: false,
      required: [
        v => !!v || 'Required Field'
      ],
      password: [
        v => (v && v.length > 6) || 'Password must be more than 6 chars'
      ],
      correo: [
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
      ]
    }
  },
  methods: {
    loginUser () {
      this.validForm = this.$refs.form.validate()
      if (this.validForm) {
        // Se puede logear
        const sendData = {
          email: this.email,
          password: this.passwordUser
        }
        const url = '/login'
        this.$axios.post(url, sendData)
          .then((res) => {
            // eslint-disable-next-line no-console
            console.log('@@ err => ', res)
            if (res.data.token) {
              localStorage.setItem('token', res.data.token)
              this.$router.push('/principal')
              // redirigir a la siguiente pagina
            }
          })
          .catch((err) => {
            // eslint-disable-next-line no-console
            console.log('@@ err => ', err)
          })
      } else {
        alert('Algo esta mal')
      }
    },
    signupUser () {
    },
    agregar () {
      this.validFormNuevo = this.$refs.formNuevo.validate()
      if (this.validFormNuevo) {
        const sendData = {
          id: Date.now().toString(),
          email: this.emailNuevo,
          password: this.passwordUserNuevo,
          nombre: this.nombreNuevo,
          apaterno: this.apaternoNuevo,
          amaterno: this.amaternoNuevo,
          direccion: this.direccionNuevo,
          telefono: this.telefonoNuevo,
          estado: this.estadoNuevo //
        }
        const url = '/signup'
        this.$axios.post(url, sendData)
          .then((res) => {
            if (res.data.message === 'Usuario Registrado Satisfactoriamente') {
              this.$emit('evento', {
                message: res.data.message,
                color: 'green',
                type: 'success'
              })
              this.showNuevo = false
            }
          })
          .catch((err) => {
            this.$emit('evento', {
              message: 'Algo Salio Mal',
              color: 'red',
              type: 'error'
            })
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
