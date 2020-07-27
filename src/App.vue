<template>
  <div id="app">
    <Weather />
  </div>
</template>

<script>
import Weather from './components/Weather';

export default {
  name: 'App',
  components: {
    Weather,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'montseratt', sans-serif;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.75)
  );
}
</style>
