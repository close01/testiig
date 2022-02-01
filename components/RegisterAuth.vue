<template>
  <div>
    <div>
      <h2 class="mb-2 text-xl text-green-500 font-bold">
        Register
      </h2>

      <div class="mb-4">
        <!-- <label class="block mb-2 text-gray-500" for="email">Email</label> -->
        <a-form-model
          ref="ruleForm"
          :model="form"
          :rules="rules"
          :label-col="labelCol"
          :wrapper-col="wrapperCol"
        >
          <a-form-model-item ref="user" label="" prop="user">
            <a-input
              id="txtUsername"
              v-model="form.user"
              size="large"
              placeholder="usename"
              class="border w-full px-2 py-1"
              type="text"
              style="width: 270px; margin:15px auto 0px;"
            />
          </a-form-model-item>
          <a-form-model-item
            ref="pw"
            label=""
            prop="pw"
          >
            <a-input-password
              id="txtPassword"
              v-model="form.pw"
              size="large"
              placeholder="password"
              class="border w-full px-2 py-1"
              type="password"
              style="width: 270px; margin:30px auto;"
              @blur="
                () => {
                  $refs.pw.onFieldBlur();
                }
              "
            />
            <h2>{{ users }}</h2>
          </a-form-model-item>
          <a-form-model-item label="" prop="fname">
            <a-input
              id="txtFname"
              v-model="form.fname"
              size="large"
              placeholder="Frist name"
              class="border w-full px-2 py-1"
              type="text"
              style="width: 270px; margin:15px auto 0px;"
            />
          </a-form-model-item>
          <a-form-model-item label="" prop="lname">
            <a-input
              id="txtLname"
              v-model="form.lname"
              size="large"
              placeholder="Last name"
              class="border w-full px-2 py-1"
              type="text"
              style="width: 270px; margin:15px auto 0px;"
            />
          </a-form-model-item>
          <a-button type="primary" style="width: 270px; height:40px" @click="onchange">
            Register
          </a-button>
          <a-button type="dashed" style="width: 270px; height:40px" @click="cancel">
            cancel
          </a-button>
        </a-form-model>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data () {
    return {
      form: {
        pw: '',
        user: '',
        fname: '',
        lname: ''
      },
      o: {
        badSequenceLength: 3,
        noQwertySequences: false
      },
      users: [],
      rules: {
        pw: [
          { required: true, message: 'Please input password', trigger: 'blur' },
          { min: 6, message: 'Password length should be longer than 6', trigger: 'blur' }
          // { required: this.form.pw.validatePassword(this.form.pw), message: 'Wrong pattern password', trigger: 'blur' }
        ],
        user: [
          { required: true, message: 'Please input username', trigger: 'blur' },
          { min: 3, max: 12, message: 'should not be longer than 12', trigger: 'blur' }
        ],
        fname: [
          { required: true, message: 'Please input name', trigger: 'blur' }
        ],
        lname: [
          { required: true, message: 'Please input name', trigger: 'blur' }
        ]
      }
    }
  },
  mounted () {
    this.getData()
  },
  methods: {
    async getData () {
      const res = await axios.get('http://localhost:3033/get/users/')
      this.users = res.data
      console.log(this.users)
    },
    onlyUnique (value, index, self) {
      return self.indexOf(value) === index
    },
    onchange () {
      const pw = this.form.pw
      // user
      if (this.form.user === '') {
        this.$notification.open({
          message: 'Notification Title',
          description:
          'name.'
        })
        return false
      }
      // password
      if (this.form.pw === '') {
        this.$notification.open({
          message: 'Notification Title',
          description:
          'password.'
        })
        return false
      }
      // fristname
      if (this.form.fname === '') {
        this.$notification.open({
          message: 'Notification Title',
          description:
          'fristname.'
        })
        return false
      }
      // lastname
      if (this.form.lname === '') {
        this.$notification.open({
          message: 'Notification Title',
          description:
          'lastname.'
        })
        return false
      }
      if (this.o.badSequenceLength) {
        const lower = 'abcdefghijklmnopqrstuvwxyz'
        const upper = lower.toUpperCase()
        const numbers = '0123456789'
        // 'qwertyuiopasdfghjklzxcvbnm'
        const qwerty = 'qwertyuiopasdfghjklzxcvbnm'
        const start = this.o.badSequenceLength - 1
        let seq = '_' + pw.slice(0, start)
        for (let i = start; i < pw.length; i++) {
          seq = seq.slice(1) + pw.charAt(i)
          if (
            lower.includes(seq) || upper.includes(seq) || numbers.includes(seq) || (this.o.noQwertySequences && qwerty.includes(seq))
          ) {
            console.log('1')
            // console.log(this.users.includes(this.form.user), typeof this.users)
            return this.o.message1
          }
        }
      }
      if (this.users.includes(this.form.user)) {
        console.log('3')
        this.$notification.open({
          message: 'Notification Title',
          description:
          'Thion.'
        })
        // return false
        return this.o.message1
      }
      console.log('2')
      // return this.o.message2,
      this.$axios.post('http://localhost:3033/post/user/', this.form)
        .then(() => {
        })
      this.$fire.auth.createUserWithEmailAndPassword(
        this.form.user,
        this.form.pw
      )
      this.$router.replace('login')
      this.$router.replace('/')
    },
    cancel () {
      this.$router.replace('/')
    }
  }
}

</script>
