<template>
  <div>
    HOLA A TODOS
  </div>
</template>

<script>
export default {
  mounted () {
    const token = localStorage.getItem('token')
    if (!token) {
      this.$router.push('/')
    }
  }
}
</script>
