<template>
  <div id="app">
    <div class="simon">
      <h1>Simon Says</h1>

      <Panel />
    </div>
  </div>
</template>

<script>
import Panel from './components/Panel'

export default {
  name: 'App',
  components: {
    Panel
  }
}
</script>

<style lang="scss">
.simon {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
