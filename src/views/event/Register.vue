<template>
  <div v-if="event">
    <p>Register for event</p>
    <button @click="register">Register Me</button>
  </div>
</template>

<script>
export default {
  props: ['event'],
  methods: {
    register() {
      this.$router.push({
        name: 'EventDetails'      })
    }
  }
}
</script>
