<template>
  <div>
    <BaseLoading v-if="isLoading"/>
    <template v-if="profileData !== null">
      <MainBlock :profile-data="profileData"/>
    </template>
  </div>
</template>

<script>
import BaseLoading from '@/components/BaseLoading'
import MainBlock from './MainBlock/Index'

import setError from '@/mixins/setError'
import { getApiAccount } from '@/api/search'

export default {
  name: 'ProfileView',
  mixins: [
    setError
  ],
  components: {
    BaseLoading,
    MainBlock
  },
  data () {
    return {
      profileData: null,
      isLoading: false
    }
  },
  created () {
    this.isLoading = true
    this.fetchData()
  },
  methods: {
    fetchData () {
      const { region, battleTag: account } = this.$route.params

      getApiAccount({ region, account })
        .then(({ data }) => {
          this.profileData = data
        })
        .catch((err) => {
          this.profileData = null
          const errObj = {
            routeParams: this.$route.params,
            message: err.message
          }

          if (err.response) {
            errObj.data = err.response.data
            errObj.status = err.response.status
          }

          this.setApiErr(errObj)

          this.$router.push({ name: 'Error' })
        })
        .finally(() => {
          this.isLoading = false
        })
    }
  }
}
</script>
