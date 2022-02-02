<template>
  <div>
    <!-- <MyProfile @authstatus="setAuthenticated"/> -->
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
      <a-card title="" :bordered="false" style="width: 300px">
        <p>frist name {{ file.fname }}</p>
        <p>last name {{ file.lname }}</p>
        <p>username {{ file.user }}</p>
        <p>password  <a @click="showModalpass">Change password</a></p>
        <a-modal v-model="visiblepass" title="Change password" @ok="handleOkPass">
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
            <!-- <a-button @click="onchangepw">
              OK
            </a-button> -->
          </a-form-model>
        </a-modal>
      </a-card>
    </div>
    <div>
      <a-button type="dashed" style="width: 270px; height:40px" @click="showModal">
        <a-icon type="edit" />
        edit profile
      </a-button>
      <a-modal v-model="visible" title="Edit profile" @ok="handleOkedit">
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
        </a-form-model>
      </a-modal>
    </div>
    <a-button type="primary" style="width: 270px; height:40px" @click="logout">
      logout
    </a-button>
    <router-view @authenticated="setAuthenticated" />
  </div>
</template>

<script>
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
  name: 'ProfilePage',
  data () {
    return {
      // authenticated: false
      authenticated: true,
      previewVisible: false,
      previewImage: '',
      visible: false,
      visiblepass: false,
      // openchang: false,
      file: {
        pw: '',
        user: '',
        fname: '',
        lname: '',
        id: '',
        auth: ''
      },
      id: this.$route.params.id,
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
<style>
.center{}
</style>
