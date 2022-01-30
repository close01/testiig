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
          <a-form-model-item label="" prop="user">
            <a-input
              id="txtUsername"
              v-model="form.user"
              max-length="12"
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
            <h2>{{ o.message }}</h2>
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
        </a-form-model>
      </div>
    </div>
  </div>
</template>
<script>
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
        badSequenceLength: 6,
        noQwertySequences: false,
        a: false,
        b: true,
        message1: '1',
        message2: '2'
      },
      rules: {
        pw: [
          { required: true, message: 'Please input password', trigger: 'blur' },
          { min: 6, message: 'Password length should be 6', trigger: 'blur' }
          // { required: this.form.pw.validatePassword(this.form.pw), message: 'Wrong pattern password', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // confirm () {
    // }
    onchange () {
      const pw = this.form.pw
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
            return this.o.message1
          }
        }
      };
      console.log('2')
      return this.o.message2
    },
    onSubmit () {
    }
  }
}

</script>
