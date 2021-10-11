<template>
  <Header />
  <Countries />
</template>

<script>
import Header from '../components/Header.vue'
import Countries from '../components/Countries.vue'

export default {
  name: 'Home',
  components: {Header,Countries}
}
</script>
