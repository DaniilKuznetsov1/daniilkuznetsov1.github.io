<template>
  <div class="rowTableRev">
	<table class="tableReview" v-if="in_revStatus < 2">
      <colgroup>
        <col style="width: 9%">
        <col style="width: 19%">
        <col style="width: 42%">
        <col style="width: 14%">
        <col style="width: 16%">
      </colgroup>
      <tr>
        <td><img v-if="in_photoCont != ''" :src="in_photoCont" width="70" height="auto" object-fit="scale-down"></td> 
        <td>{{ in_gadgetName }}</td> 
        <td>
          <table class="tdContentRev">
            <colgroup>
              <col style="width: 33%">
              <col style="width: 33%">
              <col style="width: 34%">
            </colgroup>
            <tr>
              <td><star-info :starballs="in_stars" /></td><td colspan="2"></td>
            </tr>
            <tr class="silverColor">
              <td>Впечатление: <b>{{ in_impressions }}</b></td> <td>Соответствие: <b>{{ in_correspondence }}</b></td> <td>Надёжность: <b>{{ in_reliability }}</b></td>
            </tr>
            <tr>
              <td colspan="3">{{ in_reviewText }}</td>
            </tr>
            <tr v-show="in_advFlag == true">
              <td colspan="3"><div class="stateDop">{{ stateAdv }} <i v-if="in_showAdv == true" class="fa fa-chevron-down" aria-hidden="true"></i>
                  <i v-if="in_showAdv == false" class="fa fa-chevron-up" aria-hidden="true"></i> </div></td>
            </tr>
          </table>
        </td> 
        <td class="silverColor">
        <div class="stateRev" v-bind:style="{backgroundColor: statusColor}">{{ stateOtz }}</div>
          <p class="dates">{{ date_otz | filterDate }}</p>
        </td> 
        <td><button class="btn btn-outline-secondary btn-sm" name="addDopReview" style="font-size: 13px"
        title="Дополнить отзыв" v-on:click="addDopReview()" >Дополнить отзыв</button> 
        </td> 
      </tr>
    </table>
    <table class="tableReview" v-if="in_revStatus == 2">
      <colgroup>
        <col style="width: 9%">
        <col style="width: 19%">
        <col style="width: 42%">
        <col style="width: 14%">
        <col style="width: 16%">
      </colgroup>
      <tr>
        <td>&nbsp;</td> 
        <td>&nbsp;</td> 
        <td>
          <table class="tdContentRev">
            <colgroup>
              <col style="width: 100%">
            </colgroup>
            <tr>
              <td>{{ in_reviewText }}</td>
            </tr>
            <tr>
              <td><img v-if="in_photoCont != ''" :src="in_photoCont" width="70" height="auto" object-fit="scale-down"></td>
            </tr>
          </table>
        </td> 
        <td class="silverColor">
        <div class="stateRev" v-bind:style="{backgroundColor: statusColor}">{{ stateOtz }}</div>
          <p class="dates">{{ date_otz | filterDate }}</p>
        </td> 
        <td>&nbsp;</td> 
      </tr>
    </table>
    
  </div>
</template>

<script>
import starInfo from './starInfo.vue';

/* vaule = {
   closeCode: 0,
   photoPath: this.photoPath,
   gadgetName: this.gadgetName,
   reviewText: this.reviewText,
   stars: this.params.stars, impressions: this.params.impressions, 
   correspondence: this.params.correspondence, reliability: this.params.reliability,
   photoContent: this.photoContent,
   revStatus: 0
} */

export default {
  name: 'rowTableReview',
  components: {
    'star-info':starInfo
  },
  props: {
    photoCont: String,
    gadgetName: String,
    reviewText: String,
    stars: Number,
    impressions: Number,
    correspondence: Number,
    reliability: Number,
    revStatus: Number,//0 - активный отзыв, 1 - неактивный отзыв, 2 - доп отзыв (текст строка и фото)
    revStrState: Number,//Статус отзыва (0 - опубликован, 1 - на модерации, 2 - отказ в публикации)
    rowIndex: Number,//Индекс строки компонента (индекс в массиве)
    parentIndex: Number,//Индекс родителя (-1 для основного отзыва)
    advFlag: Boolean,//Флаг "далее доп отзывы к основному" - ставится у родителя, если есть допы. У допов = false
    showAdv: Boolean,//Флаг показа доп отзывов текущего компонента
  },
  data() {
    return {
      in_photoCont: this.photoCont,
      in_gadgetName: this.gadgetName,
      in_reviewText: this.reviewText,
      in_stars: this.stars,
      in_impressions: this.impressions,
      in_correspondence: this.correspondence,
      in_reliability: this.reliability,
      in_revStatus: this.revStatus,
      in_revStrState: this.revStrState,
      in_rowIndex: this.rowIndex,//Индекс строки компонента (индекс в массиве)
      in_parentIndex: this.parentIndex,//Индекс родителя (-1 для основного отзыва)
      in_advFlag: false,//Флаг "далее доп отзывы к основному" - ставится у родителя, если есть допы. У допов = false
      in_showAdv: true, //Флаг показа доп отзывов текущего компонента
      date_otz: new Date(),
      stateOtz: 'ОПУБЛИКОВАН',//'НА МОДЕРАЦИИ', 'НЕ ОПУБЛИКОВАН'
      stateAdv: 'Подробнее',//'Подробнее'  'Скрыть'
      statusColor: '#50AD01', //Зелёный background: #50AD01; Светло зелёный background: #50AD01; Серый background: #A0A0A0;
    }
  },
  mounted() {
    
  },
  beforeDestroy() {
    
  },
  filters: {
    filterDate(val) {
      return val.toLocaleDateString();
    }
  },
  watch: {
    /*emploerUserName: function (newValue) {
        this.emploerUserName = newValue;
        this.getIntervals();
    }*/
    photoCont: function (newVal) {
      this.in_photoCont = newVal;
    },
    gadgetName: function (newVal) {
      this.in_gadgetName = newVal;
    },
    reviewText: function (newVal) {
      this.in_reviewText = newVal;
    },
    stars: function (newVal) {
      this.in_stars = newVal;
    },
    impressions: function (newVal) {
      this.in_impression = newVal;
    },
    correspondence: function (newVal) {
      this.in_correspondence = newVal;
    },
    reliability: function (newVal) {
      this.in_reliability = newVal;
    },
    revStatus: function (newVal) {
      this.in_revStatus = newVal;
      //0 - активный отзыв, 1 - неактивный отзыв, 2 - доп отзыв (текст строка и фото)
      if (this.this.in_revStatus == 1) {
        this.statusColor = '#A0A0A0';
      }
    },
    revStrState: function (newVal) {
      this.in_revStrState = newVal;
      //statusColor: '#50AD01', //Зелёный background: #50AD01; Светло зелёный background: #50AD01; Серый background: #A0A0A0;
      //Статус отзыва (0 - опубликован, 1 - на модерации, 2 - отказ в публикации)
      if (this.in_revStrState == 0) {
        this.statusColor = '#50AD01';
        this.stateOtz = 'ОПУБЛИКОВАН';
      } else if (this.in_revStrState == 1) {
        this.statusColor = '#50AD01';
        this.stateOtz = 'НА МОДЕРАЦИИ';
      } else {
        this.statusColor = '#A0A0A0';
        this.stateOtz = 'НЕ ОПУБЛИКОВАН';
      }
    },
    rowIndex: function (newVal) {
      this.in_rowIndex = newVal;
    },
    parentIndex: function (newVal) {
      this.in_parentIndex = newVal;
    },
    advFlag: function (newVal) {
      this.in_advFlag = newVal;
    },
    showAdv: function (newVal) {
      this.in_showAdv = newVal;
      //stateAdv: 'Подробнее',//'Подробнее'  'Скрыть'
      if (this.in_showAdv == true) {
        this.stateAdv = 'Скрыть';
      } else {
        this.stateAdv = 'Подробнее';
      }
    }
  },
  methods: {
    addDopReview() {
      this.$emit('adddop');
    }
  }
}
</script>

<style scoped>
  .rowTableRev {
    display:block;
  }
  .tableReview {
    box-sizing: border-box;
    width: 100%;
  }
  .btToolStatus {
    border: 1px solid #979797;
    border-radius: 2px;
    color: #979797;
    background-color: #FFFFFF;
    font-size: 13px;
  }
  .tdContentRev {
    box-sizing: border-box;
    width: 100%;
    font-size: 12px;
  }
  .silverColor {
    color: #AFAFAF; font-size: 12px;
  }
  .stateDop {
    font-size: 11px;
    color: #FF6D00;
  }
  .dates {
    padding-left: 5%; padding-right: 5%;
    text-align: center;
  }
  .stateRev {
    font-size: 10px;
    line-height: 14px;
    color: #FFFFFF;
    border-radius: 2px;
    margin-left: 5%; margin-right: 5%;
    padding: 4px 8px 4px 8px;
    text-align: center;
  }
</style>