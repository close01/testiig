<template>
  <LoginForm @authenticated="setAuthenticated"/>
</template>

<script>
export default {
  name: 'IndexPage',
  data () {
    return {
    }
  },
  mounted () {

  },
  methods: {
    setAuthenticated (status) {
      console.log('auth indexpage ', status)
    }
  }
}
</script>
