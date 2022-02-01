<template>
  <div>
    <h1>Profile</h1>
    <MyProfile @authstatus="setAuthenticated"/>
    <a-button type="primary" style="width: 270px; height:40px" @click="logout">
      logout
    </a-button>
    <router-view @authenticated="setAuthenticated" />
  </div>
</template>

<script>
export default {
  name: 'ProfilePage',
  data () {
    return {
      // authenticated: false
      authenticated: true
    }
  },
  mounted () {
    if (!this.authenticated) {
      this.$router.replace('/')
    }
  },
  methods: {
    setAuthenticated (status) {
      this.authenticated = status
      console.log('authenticated1111', this.authenticated)
      console.log('tatus', status)
      console.log('authenticated', this.authenticated)
    },
    logout () {
      this.authenticated = false
      this.$router.replace('/')
    }
  }
}
</script>
