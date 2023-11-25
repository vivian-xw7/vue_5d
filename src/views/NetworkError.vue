<template>
  <h1>Oops!</h1>
  <h3>It looks like you're experiencing some network issues. Please <a href="#" @click="$router.go(-1)">click here</a> to try again.</h3>
</template>

<!-- <script>
export default {
  props: {
    resource: {
      type: String,
      required: true,
      default: 'page'
    }
  }
}
</script> -->
