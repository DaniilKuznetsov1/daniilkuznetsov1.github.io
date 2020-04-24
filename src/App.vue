<template>
  <div id="app">
    <main1 />
  </div>
</template>

<script>
import main1 from './components/main.vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

export default {
  name: 'App',
  components: {
    main1
  }
}
</script>

<style>
@import './assets/css/main.css';
@import './assets/css/font-awesome.css';

html {
  width: 100%; height: 100%;
}
body {
  width: 100%; height: 100%;
  margin: 0px 0px 0px 0px;
  padding: 0px 0px 0px 0px;
}
#app {
  display: block; width: 100%; height: 100%;
  font-family: 'Open Sans';
}

@media (min-width: 1000px)
{
  .container1 {
    margin-top: 131px;  margin-bottom: 308px;
	margin-left: 305px; margin-right: 305px;
	padding: 0px 0px 0px 0px;
	width: 70%; height: 80%;
  }
}
@media (max-width: 1000px) and (min-width: 400px)
{
  .container1 {
    margin-top: 65px;  margin-bottom: 150px;
	margin-left: 140px; margin-right: 140px;
	padding: 0px 0px 0px 0px;
	width: 70%; height: 80%;
  }
}
@media (max-width: 400px) 
{
  .container1 {
    margin-top: 71px;  margin-bottom: 240px;
	margin-left: 20px; margin-right: 20px;
	padding: 0px 0px 0px 0px;
	width: 90%; height: 80%;
  }
}
</style>
