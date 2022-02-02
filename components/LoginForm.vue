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
      profile2: [],
      encryptP: null
    }
  },
  mounted () {
    this.getProfile()
    this.getProfile1()
    this.getProfile2()
  },
  methods: {
    async getProfile () {
      const res = await axios.get('http://localhost:3034/get/users')
      this.profile = res.data
      console.log('username', this.profile)
      // console.log(this.input.username)
    },
    async getProfile1 () {
      const res = await axios.get('http://localhost:3034/get/profiles/pw')
      this.profile1 = res.data
      console.log('password encrypt', this.profile1)
      // console.log(this.input.username)
    },
    async getProfile2 () {
      const res = await axios.get('http://localhost:3034/get/profiles/id')
      this.profile2 = res.data
      console.log('id', this.profile2)
      // console.log(this.input.username)
    },
    login () {
      // const inputU = this.input.username
      // const inputP = this.input.password
      // const encrypt = axios.get('http://localhost:3034/pass/', this.input.password)
      // console.log(encrypt)
      // const encrypt = await axios.get('http://localhost:3034/pass/', this.profile1[this.profile.indexOf(inputU)])
      // console.log('eeeeeee', encrypt)
      // this.encryptP = encrypt.data
      // console.log('eeeeeeeedfdfdfdfdfdfd', this.profile1[this.profile.indexOf(inputU)])
      const baseString = this.profile1[this.profile.indexOf(this.input.username)]
      console.log('baseString', baseString)
      const CryptoJS = require('crypto-js')
      const decipher = CryptoJS.AES.decrypt(baseString, 'CIPHERKEY')
      console.log('decipher', decipher)
      const plaintext = decipher.toString(CryptoJS.enc.Utf8)
      console.log('planintext', plaintext)
      // //////////////////////////////////////////
      if (this.input.username !== '' && this.input.password !== '') {
        this.$emit('authenticated', true)
        if (this.profile.includes(this.input.username) || this.profile1.includes(this.input.password)) {
          console.log(this.profile2[this.profile.indexOf(this.input.username)])
          if (this.input.username === this.profile[this.profile.indexOf(this.input.username)] && this.input.password === plaintext) {
            // if (this.input.username === this.profile[this.profile.indexOf(this.input.username)] && this.input.password === this.profile1[this.profile.indexOf(this.input.username)]) {
            // true ค่าไม่ส่ง
            // this.$emit('authenticated', false)
            console.log('pass', plaintext)
            this.$router.replace(`/profile/${this.profile2[this.profile.indexOf(this.input.username)]}`)
            // ////////////////////////////////
          } else {
            this.$notification.open({
              message: 'Notification Title',
              description:
          'The username and / or password is incorrect'
            })
            console.log('pass', plaintext)
            console.log('The username and / or password is incorrect')
          }
        } else {
          this.$notification.open({
            message: 'Notification Title',
            description:
          'A username and password must be present'
          })
          console.log('A username and password must be present')
        }
      }
    }
  }
}
</script>
