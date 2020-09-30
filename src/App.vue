<template>
  <div id="app">
    <AppForm />
  </div>
</template>

<script>
import AppForm from './AppForm.vue';

export default {
  components: {
    AppForm,
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900');

$primary-color: #c6538c;
$error-color: #df2e2e;
$text-color: #020202;
$text-color-light: #fefefe;

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Roboto', sans-serif;
  font-weight: 500;
  font-size: 1.2rem;
  background: #fefefe;
}

.error {
  background: $error-color;
  color: $text-color-light;
  padding: 3rem;
  position: absolute;
  right: 2rem;
}
</style>
