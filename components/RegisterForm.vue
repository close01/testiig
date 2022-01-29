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
          <a-form-model-item label="" prop="username">
            <a-input
              id="txtUsername"
              v-model="form.username"
              max-length="12"
              size="large"
              placeholder="usename"
              class="border w-full px-2 py-1"
              type="text"
              style="width: 270px; margin:15px auto 0px;"
            />
          </a-form-model-item>
        </a-form-model>
      </div>
      <div class="mb-4">
        <!-- <label class="block mb-2 text-gray-500" for="password">Password</label> -->
        <a-form-model-item ref="pw" label="" prop="pw">
          <a-input-password
            id="txtPassword"
            v-model="password.pw"
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
        </a-form-model-item>
      </div>
      <div class="mb-4">
        <!-- <label class="block mb-2 text-gray-500" for="email">Email</label> -->
        <a-form-model-item label="" prop="fname">
          <a-input
            id="txtFname"
            v-model="form.fname"
            max-length="12"
            size="large"
            placeholder="Frist name"
            class="border w-full px-2 py-1"
            type="text"
            style="width: 270px; margin:15px auto 0px;"
          />
        </a-form-model-item>
      </div>
      <div class="mb-4">
        <!-- <label class="block mb-2 text-gray-500" for="email">Email</label> -->
        <a-form-model-item label="" prop="lname">
          <a-input
            id="txtLname"
            v-model="form.lname"
            max-length="12"
            size="large"
            placeholder="Last name"
            class="border w-full px-2 py-1"
            type="text"
            style="width: 270px; margin:15px auto 0px;"
          />
        </a-form-model-item>
      </div>
      <a-button type="primary" style="width: 270px; height:40px" href="/register">
        Register
      </a-button>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      password: {
        pw: '',
        username:'',
        fname:'',
        l
      },
      rules: {
        pw: [
          { required: true, message: 'Please input Activity name', trigger: 'blur' },
          { min: 6, max: 20, message: 'Length should be 6 to 20', trigger: 'blur' }
        ]
      }
    }
  }
}

</script>
