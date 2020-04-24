<template>
  <div class="container1">
    <h2 class="header">Мои отзывы о товарах</h2>
	<div class="info"> <span class="circle orange">!</span> Ваши отзывы будут опубликованы на сайте после проверки модератором.</div>
	<review-table />
  </div>
</template>

<script>
import reviewTable from './reviewTable.vue';

export default {
  name: 'main1',
  components: {
    'review-table': reviewTable
  },
  props: {
    
  },
  data() {
    return {
      tables:'',

    }
  },
  mounted() {
    
  },
  beforeDestroy() {
    
  },
  methods: {
    
  }
}
</script>

<style scoped>
  .header {
    font-style: normal;
    font-weight: bold;
    font-size: 20px;
	color: #383838;
	margin-top: 0px; margin-bottom: 22px;
  }
  .info {
    background-color: #F7F7F7;
	color: #383838;
	padding: 30px 305px 31px 50px;
	margin-bottom: 22px;
  }
  .circle {
    display: inline-block; 
	position: relative;
	border-radius: 7px;
	text-align: center;
	line-height: 14px;
	width: 15px; height: 15px;
	top: -2px;
  }
  .orange {
    background-color: #FF6D00;
	color: #FFFFFF; 
	font-size: 10px;
	margin-left: 5px; margin-right: 15px;
  }
</style>
