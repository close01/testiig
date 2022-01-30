<template>
  <div>
    <div class="clearfix">
      <a-upload
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
        <img alt="example" style="width: 100%" :src="previewImage">
      </a-modal>
    </div>
    <div style="">
      <a-card title="Profile" :bordered="false" style="width: 300px">
        <p>frist name {{ "aaaaaaaaaaaaaaaa" }}</p>
        <p>last name {{ "aaaaaaaaaaaaaaaa" }}</p>
        <p>username {{ "bbbbbbbbbbb" }}</p>
        <p>password  <a @click="showModalpass">Change password</a></p>
        <a-modal v-model="visiblepass" title="Change password" @ok="handleOk">
          <a-form-model
            ref="ruleForm"
            v-model="openchange"
            :model="formchange"
            :rules="ruleschange"
            :label-col="labelCol"
            :wrapper-col="wrapperCol"
          >
            <a-form-model-item
              ref="pwc"
              label=""
              prop="pwc"
            >
              <div id="inputnewpwc">
                <a-input-password
                  id="txtPassword"
                  v-model="formchange.pwc"
                  size="large"
                  placeholder="new password"
                  class="border w-full px-2 py-1"
                  type="password"
                  style="width: 270px; margin:30px auto;"
                  @blur="
                    () => {
                      $refs.pwc.onFieldBlur();
                    }
                  "
                />
              </div>
            </a-form-model-item>
            <a-button @click="onchangepw">
              OK
            </a-button>
          </a-form-model>
        </a-modal>
      </a-card>
    </div>
    <div>
      <a-button type="primary" @click="showModal">
        <a-icon type="edit" />
        edit profile
      </a-button>
      <a-modal v-model="visible" title="Edit profile" @ok="handleOk">
        <a-form-model
          ref="ruleForm"
          :model="editform"
          :rules="rulesedit"
          :label-col="labelCol"
          :wrapper-col="wrapperCol"
        >
          <a-form-model-item label="" prop="user">
            <a-input
              id="txtUsername"
              v-model="editform.user"
              max-length="12"
              size="large"
              placeholder="new usename"
              class="border w-full px-2 py-1"
              type="text"
              style="width: 270px; margin:15px auto 0px;"
            />
          </a-form-model-item>
          <a-form-model-item label="" prop="user">
            <a-input
              id="txtUsername"
              v-model="editform.fname"
              max-length="12"
              size="large"
              placeholder="new frist name"
              class="border w-full px-2 py-1"
              type="text"
              style="width: 270px; margin:15px auto 0px;"
            />
          </a-form-model-item>
          <a-form-model-item label="" prop="user">
            <a-input
              id="txtUsername"
              v-model="editform.lname"
              max-length="12"
              size="large"
              placeholder="new last name"
              class="border w-full px-2 py-1"
              type="text"
              style="width: 270px; margin:15px auto 0px;"
            />
          </a-form-model-item>
          <!-- <a-button type="primary" style="width: 270px; height:40px" @click="onchange">
            Register
          </a-button> -->
        </a-form-model>
      </a-modal>
    </div>
  </div>
</template>
<script>
function getBase64 (file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.readAsDataURL(file)
    reader.onload = () => resolve(reader.result)
    reader.onerror = error => reject(error)
  })
}
export default {
  data () {
    return {
      previewVisible: false,
      previewImage: '',
      visible: false,
      visiblepass: false,
      // openchang: false,
      formchange: {
        pwc: ''
      },
      editform: {
        user: '',
        fname: '',
        lname: ''
      },
      o: {
        badSequenceLength: 3,
        noQwertySequences: false
        // a: false,
        // b: true,
        // message1: '1',
        // message2: '2'
      },
      ruleschange: {
        pwc: [
          { required: true, message: 'Please input password', trigger: 'blur' },
          { min: 6, message: 'Password length should be 6', trigger: 'blur' }
        ]
      },
      fileList: [
        {
          uid: '-1',
          name: 'image.png',
          status: 'done',
          url: 'https://zos.alipayobjects.com/rmsportal/jkjgkEfvpUPVyRjUImniVslZfWPnJuuZ.png'
        }
      ]
    }
  },
  methods: {
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
    },
    // showModal () {
    //   this.openchange = true
    // },
    showModal () {
      this.visible = true
    },
    showModalpass () {
      this.visiblepass = true
    },
    handleOk (e) {
      // console.log(e)
      this.visible = false
      this.visiblepass = false
    },
    onchangepw () {
      const pwc = this.formchange.pwc
      if (this.o.badSequenceLength) {
        const lower = 'abcdefghijklmnopqrstuvwxyz'
        const upper = lower.toUpperCase()
        const numbers = '0123456789'
        // 'qwertyuiopasdfghjklzxcvbnm'
        const qwerty = 'qwertyuiopasdfghjklzxcvbnm'
        const start = this.o.badSequenceLength - 1
        let seq = '_' + pwc.slice(0, start)
        for (let i = start; i < pwc.length; i++) {
          seq = seq.slice(1) + pwc.charAt(i)
          if (
            lower.includes(seq) || upper.includes(seq) || numbers.includes(seq) || (this.o.noQwertySequences && qwerty.includes(seq))
          ) {
            console.log('1')
            return this.o.message1
          }
        }
      }
      console.log('2')
      this.openchang = false
      return this.o.message2
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
</style>
