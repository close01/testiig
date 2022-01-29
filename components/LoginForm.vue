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

            size="large"
            placeholder="password"
            class="border w-full px-2 py-1"
            type="password"
            style="width: 270px; margin:30px auto;"
          />
        </div>
        <div>
          <a-button type="primary" style="width: 270px; height:40px">
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
