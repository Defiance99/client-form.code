<template>
  <div id="app">
    <h1>Vue App</h1>
    <hr>
    <FormCreateClient></FormCreateClient>
  </div>
</template>

<script>
import FormCreateClient from '@/components/form-create-client';

export default {
  name: 'App',
  components: {
    FormCreateClient
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
