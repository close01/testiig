<template>
  <div>
    <h1>Profile</h1>
    <MyProfile />
    <router-link v-if="authenticated" to="/login" v-on:click.native="logout()" replace>Logout</router-link>
    <a-button type="dashed" style="width: 270px; height:40px" @click="logout">
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
      authenticated: true
      // mockAccount: {
      //   username: 'aaaaaa',
      //   password: 'aaaaaa'
      // }
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
      console.log('xzxzxzxz', status)
    },
    logout () {
      this.authenticated = false
      this.$router.replace('/')
    }
  }
}
</script>
