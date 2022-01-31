<template>
  <div>
    <div>
      <form>
        <h2 class="mb-2 text-xl text-green-500 font-bold">
          Login
        </h2>

        <div class="mb-4">
          <!-- <label class="block mb-2 text-gray-500" for="email">Email</label> -->
          <a-input
            id="txtEmail"
            v-model="input.username"
            size="large"
            placeholder="usename"
            class="border w-full px-2 py-1"
            type="text"
            style="width: 270px; margin:15px auto 0px;"
          />
          {{ profile }}
          <!-- {{profile.u.indexof(input.username)}} -->
          {{ typeof input.username }}
        </div>
        <div class="mb-4">
          <!-- <label class="block mb-2 text-gray-500" for="password">Password</label> -->
          <a-input-password
            id="txtPassword"
            v-model="input.password"
            size="large"
            placeholder="password"
            class="border w-full px-2 py-1"
            type="password"
            style="width: 270px; margin:30px auto;"
          />
          {{ input.password }}
          <!-- {{this.$parent.mockAccount.username}} -->
        </div>
        <div>
          <a-button type="primary" style="width: 270px; height:40px" @click="login">
            Login
          </a-button>
        </div>
        <div>
          <a-button type="primary" style="width: 270px; height:40px" href="/register">
            Register
          </a-button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data () {
    return {
      input: {
        username: '',
        password: ''
      },
      profile: [],
      profile1: []
    }
  },
  mounted () {
    this.getProfile()
    this.getProfile1()
  },
  methods: {
    async getProfile () {
      const res = await axios.get('http://localhost:3033/get/users')
      this.profile = res.data
      console.log('uuu', this.profile)
      // console.log(this.input.username)
    },
    async getProfile1 () {
      const res = await axios.get('http://localhost:3033/get/profiles/pw')
      this.profile1 = res.data
      console.log('ppp', this.profile1)
      // console.log(this.input.username)
    },
    login () {
      if (this.input.username !== '' && this.input.password !== '') {
        if (this.profile.includes(this.input.username) || this.profile1.includes(this.input.password)) {
          if (this.input.username === this.profile.user && this.input.password === '') {
            this.$emit('authenticated', true)
            this.$router.replace('/myprofile')
          } else {
            console.log('The username and / or password is incorrect')
          }
        } else {
          console.log('A username and password must be present')
        }
      }
    }
  }
}
</script>
