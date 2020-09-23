<template>
  <div id="app">
    <Registration>
      <label class="label" slot="name">Name</label>
      <label class="label" slot="sername">Sername</label>
      <label class="label" slot="country">Country</label>
      <label class="label" slot="file">File</label>
    </Registration>
  </div>
</template>

<script>
import Registration from './components/Registration'

export default {
  name: "app",
  components: {
    Registration,
  },
};
</script>

<style lang="scss">

</style>
