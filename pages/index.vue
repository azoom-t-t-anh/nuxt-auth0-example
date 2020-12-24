<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        Test cloud-endpoint using JWT
      </h1>
      <div class="links">
        <el-button v-if="isCallWiththoutToken" :loading="isCallWiththoutTokenLoading" type="warning" @click="callWithoutToken">
          Call protected API without token
        </el-button>
        <el-button v-if="isRequireTokenVisible" :loading="isLoading" type="primary" @click="getToken">
          Get token from API
        </el-button>
        <el-button v-if="isCallAPIVisible" :loading="isLoading" type="success" @click="callAPI">
          Call API
        </el-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isLoading: false,
      isCallWiththoutTokenLoading: false,
      isCallWiththoutToken: true,
      isRequireTokenVisible: true,
      isCallAPIVisible: false
    }
  },
  methods: {
    async callWithoutToken () {
      try {
        this.isCallWiththoutTokenLoading = true
        const response = await this.$axios.$get('/endpoint-api')
        this.$notify({
          type: 'success',
          title: 'Call API Status',
          message: response
        })
        this.isCallWiththoutTokenLoading = false
      } catch (error) {
        this.isCallWiththoutTokenLoading = false
        const { response } = error
        this.$notify({
          type: 'error',
          title: 'Call API Status',
          message: response.data.message
        })
      }
    },
    async getToken () {
      try {
        this.isLoading = true
        const response = await this.$axios.$get('/cloud-endpoint/token')
        const accessToken = response.accessToken.access_token
        this.$axios.setToken(accessToken, 'Bearer')
        this.$notify({
          type: 'success',
          title: 'Access Token',
          message: accessToken
        })
        this.isCallAPIVisible = true
        this.isCallWiththoutToken = false
      } catch (error) {
        this.$notify({
          type: 'error',
          title: 'Access Token',
          message: error
        })
      } finally {
        this.isLoading = false
        this.isRequireTokenVisible = false
      }
    },
    async callAPI () {
      try {
        this.isLoading = true
        const response = await this.$axios.$get('/endpoint-api')
        this.$notify({
          type: 'success',
          title: 'Call API Status',
          message: response
        })
        this.isLoading = false
      } catch (error) {
        this.$notify({
          type: 'error',
          title: 'Call API Status',
          message: error
        })
      }
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 1.2rem;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
