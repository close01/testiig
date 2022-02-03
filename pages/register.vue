<template>
  <div class="center" style="padding-top: 20px;">
    <h2 class="mb-2 text-xl text-green-500 font-bold">
      Register
    </h2>
    <div class="clearfix">
      <a-upload
        ref="input1"
        action="https://www.mocky.io/v2/5cc8019d300000980a055e76"
        list-type="picture-card"
        :file-list="fileList"
        @preview="handlePreview"
        @change="handleChange"
      >
        <div v-if="fileList.length < 1">
          <a-icon type="plus" />
          <div class="ant-upload-text">
            Upload
          </div>
        </div>
      </a-upload>
      <a-modal :visible="previewVisible" :footer="null" @cancel="handleCancel">
        <img alt="example" style="" :src="previewImage">
      </a-modal>
    </div>
    <div>
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
              style="width: 270px; margin:15px auto 0px;"
              @blur="
                () => {
                  $refs.pw.onFieldBlur();
                }
              "
            />
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
          <div>
            <a-button type="primary" style="width: 270px; height:40px; margin-bottom: 20px; " @click="onchange">
              Register
            </a-button>
          </div>
          <div>
            <a-button type="dashed" style="width: 270px; height:40px" @click="cancel">
              cancel
            </a-button>
          </div>
        </a-form-model>
      </div>
    </div>
    <!-- <UploadProfile />
    <RegisterForm /> -->
  </div>
</template>

<script>
// import { Row, Col } from 'antd'
import axios from 'axios'
function getBase64 (file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.readAsDataURL(file)
    reader.onload = () => resolve(reader.result)
    reader.onerror = error => reject(error)
  })
}
export default {
  name: 'RegisterPage',
  data () {
    return {
      previewVisible: false,
      previewImage: '',
      fileList: [
      ],
      form: {
        pw: '',
        user: '',
        fname: '',
        lname: ''
      },
      o: {
        badSequenceLength: 4,
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
      const res = await axios.get('http://localhost:3034/get/users/')
      this.users = res.data
      // console.log(this.users)
    },
    onchange () {
      const pw = this.form.pw
      // user
      if (this.form.user === '') {
        this.$notification.open({
          message: '! notify',
          description:
          'please input username.'
        })
        return false
      }
      // password
      if (this.form.pw === '') {
        this.$notification.open({
          message: '! notify',
          description:
          'please input password.'
        })
        return false
      }
      // fristname
      if (this.form.fname === '') {
        this.$notification.open({
          message: '! notify',
          description:
          'please input fristname.'
        })
        return false
      }
      // lastname
      if (this.form.lname === '') {
        this.$notification.open({
          message: '! notify',
          description:
          'please input lastname.'
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
            // console.log('1')
            // console.log(this.users.includes(this.form.user), typeof this.users)
            return
          }
        }
      }
      if (this.users.includes(this.form.user)) {
        // console.log('3')
        this.$notification.open({
          message: '! notify',
          description:
          'username already has user'
        })
        // return false
        return
      }
      // console.log('2')
      // return this.o.message2,
      this.$axios.post('http://localhost:3034/post/user/', this.form)
        .then(() => {
          // this.$router.push(`/profile/${this.profile.userId}`);
          // console.log("put", this.profile);
        })
      this.$router.replace('/')
    },
    cancel () {
      this.$router.replace('/')
    },
    handleCancel () {
      this.previewVisible = false
    },
    async handlePreview (file) {
      if (!file.url && !file.preview) {
        file.preview = await getBase64(file.originFileObj)
      }
      this.previewImage = file.url || file.preview
      this.previewVisible = true
    },
    handleChange ({ fileList }) {
      this.fileList = fileList
      console.log('file', fileList)
    }
  }
}
</script>
<style>
/* you can make up upload button and sample style by using stylesheets */
.ant-upload-select-picture-card i {
  font-size: 32px;
  color: #999;
}

.ant-upload-select-picture-card .ant-upload-text {
  margin-top: 8px;
  color: #666;
}
.ant-upload-picture-card-wrapper {
  width: 1%;
  margin-left: -87px;
}
</style>
