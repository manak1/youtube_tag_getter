<template>
  <div class="p-container">
    <TheHeader />
    <nuxt class="pt-12 flex-grow" />
    <TheFooter />
  </div>
</template>

<script>
import TheFooter from '@/components/common/TheFooter'
import TheHeader from '@/components/common/TheHeader'
export default {
  components: {
    TheHeader,
    TheFooter
  }
}
</script>

<style lang="scss" scoped>
.p-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
</style>
