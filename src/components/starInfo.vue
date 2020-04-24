<template>
  <div class="starInd">
	<div class="starInfo">
      <span class="cstar" v-for="(star, index1) in arrStars" v-bind:key="index1">
                          <i v-if="star == 0" class="fa fa-star-o" aria-hidden="true"></i>
                          <i v-if="star == 1" class="fa fa-star" aria-hidden="true"></i>
                          <i v-if="star == 2" class="fa fa-star-half-o" aria-hidden="true"></i>
      </span>
      <b class="balls">{{ in_starballs }}</b>
    </div>
  </div>
</template>

<script>
const LEN_STARS = 5;

export default {
  name: 'starInfo',
  components: {
    
  },
  props: {
    starballs: Number
  },
  data() {
    return {
      in_starballs: 5,
      arrStars:[1,1,1,1,1], //0 - пустые звёзды, 1 - закрашенные, 2 - полузакрашенная (1 на всё) 
    }
  },
  mounted() {
    
  },
  beforeDestroy() {
    
  },
  watch: {
    /*emploerUserName: function (newValue) {
        this.emploerUserName = newValue;
        this.getIntervals();
    }*/
    starballs: function(newValue) {
      this.in_starballs = newValue;
      this.calcStars();
    },
  },
  methods: {
    calcStars() {
      let roundFloorStars = Math.floor(this.in_starballs);
      let tail = this.in_starballs - roundFloorStars;
      if (roundFloorStars <= LEN_STARS) {
        for (let i=0;i<this.arrStars.length;i++) {//Сброс массива
          this.$set(this.arrStars,i,0);
        }
        for (let i=0;i<roundFloorStars;i++) {//Установка значений
          this.$set(this.arrStars,i,1);
        }
        if (tail > 0) {
          this.$set(this.arrStars,roundFloorStars,2);
        } 
      }
      
    },
    
  }
}
</script>

<style scoped>
  .starInd {
    display:block;
  }
  .starInfo {
    display: flex; flex-direction: row; flex-wrap: nowrap; 
    justify-content: space-between;
  }
  .cstar {
    margin-left: 4px; 
    font-size: 12px;
    color: #ED8A19;
  }
  .balls {
    font-style: normal;
    font-weight: bold;
    font-size: 12px;
    color: #AFAFAF;
  }
</style>