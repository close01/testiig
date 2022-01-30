<template>
  <div>
    <h1>Profile</h1>
    <UploadProfile />
  </div>
</template>

<script>
import UploadProfile from '~/components/UploadProfile.vue'
export default {
  name: 'ProfilePage',
  components: { UploadProfile }
}
</script>
