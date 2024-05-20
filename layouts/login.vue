<template>
  <v-app>
    <v-main>
      <ui-alert
        v-if="showAlert"
        :message="message"
        :color="color"
        :type="type"
      />
      <v-row
        style="width: 100%; height: 100vh;"
        justify="center"
        align="center"
      >
        <nuxt
          @evento="cargaAlerta"
        />
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      showAlert: false,
      mensaje: '',
      color: '',
      type: ''
    }
  },
  created () {
    this.$nuxt.$on('evento', (data) => {
      this.message = data.message
      this.color = data.color
      this.type = data.type
      this.showAlert = true
      setTimeout(() => {
        this.showAlert = false
      }, data.time || 5000)
    })
  }, /// ////////////////////////////////
  methods: {
    cargaAlerta (data) {
      // eslint-disable-next-line no-console
      console.log('@@@ data => ', data)
    }
  }
}
</script>
