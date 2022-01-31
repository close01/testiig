<template>
  <div>
    <h1>Profile</h1>
    <MyProfile />
    <a-button type="dashed" style="width: 270px; height:40px" @click="logout">
      cancel
    </a-button>
    <router-view @authenticated="setAuthenticated" />
  </div>
</template>

<script>
export default {
  data () {
    return {
      authenticated: false,
      mockAccount: {
        username: 'aaaaaa',
        password: 'aaaaaa'
      }
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
    },
    logout () {
      this.authenticated = false
    }
  }
}
</script>
