<template>
  <div>
    <div>
      <form>
        <h2 class="mb-2 text-xl text-green-500 font-bold">
          Login
        </h2>

        <div class="mb-4">
          <a-input
            id="txtUser"
            v-model="input.username"
            size="large"
            placeholder="usename"
            class="border w-full px-2 py-1"
            type="text"
            style="width: 270px; margin:15px auto 0px;"
          />
        </div>
        <div class="mb-4">
          <a-input-password
            id="txtPassword"
            v-model="input.password"
            size="large"
            placeholder="password"
            class="border w-full px-2 py-1"
            type="password"
            style="width: 270px; margin:30px auto;"
          />
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
// import firebase from "firebase";
// import mapMutations from 'vuex'
export default {
  data () {
    return {
      input: {
        username: '',
        password: '',
        auth: true
      },
      profile: [],
      profile1: [],
      profile2: []
    }
  },
  mounted () {
    this.getProfile()
    this.getProfile1()
    this.getProfile2()
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
    async getProfile2 () {
      const res = await axios.get('http://localhost:3033/get/profiles/id')
      this.profile2 = res.data
      console.log('idididiid', this.profile2)
      // console.log(this.input.username)
    },
    async login () {
    //   console.log(this.formData)
      try {
        await this.$fire.auth.signInWithEmailAndPassword(
          this.input.username,
          this.input.password
        )
        this.$router.replace('home')
      } catch (e) {
        alert(e)
      }
    }
  }
}
</script>
