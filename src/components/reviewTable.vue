<template>
  <div class="rtablemain">
    <add-dialog :showdialog="showAdddialog" :revStatus="revStatus" :dialCaption="dCaption" v-on:closeDial="onAddReview" />
    <div class="tableHeader">
      <table class="tableReview">
        <colgroup>
          <col style="width: 9%">
          <col style="width: 19%">
          <col style="width: 42%">
          <col style="width: 14%">
          <col style="width: 16%">
        </colgroup>
        <tr>
          <th>Фото</th> <th>Наименование товара</th> <th>Ваши отзывы</th> <th>Статус отзыва</th> 
            <th>Действия <button class="btToolStatus" name="addReview" title="Добавить отзыв" v-on:click="addReview()">+</button> 
          <button class="btToolStatus" name="addDeactivReview" title="Добавить неактивный отзыв" v-on:click="addReview('deactive')">+</button> </th> 
        </tr>
      </table>
      </div>
    <div class="tableData">
      <row-table-review v-for="(review, index) in arrReview" :key="index" :photoCont="review.photoContent" :gadgetName="review.gadgetName" :reviewText="review.reviewText"
      :stars="review.stars" :impressions="review.impressions" :correspondence="review.correspondence" :reliability="review.reliability" 
      :revStatus="review.revStatus" :revStrState="review.revStrState" :rowIndex="review.rowIndex" :parentIndex="review.parentIndex" 
      :advFlag="review.advFlag" :showAdv="review.showAdv" @adddop="onAddDop" />
    </div>
  </div>
</template>
<script>
import addDialog from './addDialod.vue';
import rowTableReview from './rowTableReview.vue';

export default {
  name: 'reviewTable',
  components: {
    'add-dialog':addDialog,
    'row-table-review':rowTableReview
    /* photoCont: String,     gadgetName: String,    reviewText: String,    stars: Number,
    impressions: Number,    correspondence: Number,    reliability: Number,
    revStatus: Number,//0 - активный отзыв, 1 - неактивный отзыв, 2 - доп отзыв (текст строка и фото)
    revStrState: Number,//Статус отзыва (0 - опубликован, 1 - на модерации, 2 - отказ в публикации)
    rowIndex: Number,//Индекс строки компонента (индекс в массиве)
    parentIndex: Number,//Индекс родителя (-1 для основного отзыва)
    advFlag: Boolean,//Флаг "далее доп отзывы к основному" - ставится у родителя, если есть допы. У допов = false
    showAdv: Boolean,//Флаг показа доп отзывов текущего компонента */
  },
  props: {
    
  },
  data() {
    return {
      arrReview:[],
      showAdddialog: false,
      dCaption: 'Добавить отзыв',
      revStatus: 0,//0 - активный отзыв, 1 - неактивный отзыв, 2 - доп отзыв (текст строка и фото)
      
    }
  },
  mounted() {
    let arrRewStr = sessionStorage.getItem('arrReview');
    let arrRew = [];
    if (arrRewStr != undefined) {
      arrRew = JSON.parse(arrRewStr);
      this.arrReview = [];
      for (let i=0;i<arrRew.length;i++) {
        this.arrReview.push(arrRew[i]);
      }
    }
  },
  beforeDestroy() {
    
  },
  methods: {
    addReview(active) {
      if (active == undefined) {
        this.revStatus = 0;
      } else if (active == 'deactive') {
        this.revStatus = 1;
      } else if (active == 'advanced') {
        this.revStatus = 2;
      }
      this.showAdddialog = true;
    },
    /* vaule = {
        closeCode: 0,
        photoPath: this.photoPath,
        gadgetName: this.gadgetName,
        reviewText: this.reviewText,
        stars: this.params.stars, impressions: this.params.impressions, 
        correspondence: this.params.correspondence, reliability: this.params.reliability,
        photoContent: this.photoContent,
        revStatus: 0,
        revStrState: 0,//Статус отзыва (0 - опубликован, 1 - на модерации, 2 - отказ в публикации)
        rowIndex: -1,//Индекс строки компонента (индекс в массиве)
        parentIndex: -1,//Индекс родителя (-1 для основного отзыва)
        advFlag: false,//Флаг "далее доп отзывы к основному" - ставится у родителя, если есть допы. У допов = false
        showAdv: false,//Флаг показа доп отзывов текущего компонента
      } */
    onAddReview(value) {
      this.showAdddialog = false;
      //console.log(value);
      if (value.closeCode == 0) {
        this.arrReview.push(value);
        let lastInd = this.arrReview.length - 1;
        this.arrReview[lastInd].rowIndex = lastInd;
      
        sessionStorage.setItem('arrReview', JSON.stringify(this.arrReview));
      }
    },
    onAddDop() {
      this.addReview('advanced');
    }
  }
}
</script>
<style scoped>
.tableHeader {
  display: block;
  border: 1px solid #F2F2F2;
  box-sizing: border-box;
  overflow-y: scroll;
}
.tableData {
  display: block; 
  box-sizing: border-box;
  overflow-y: scroll;
  min-height: 200px;
}
.btToolStatus {
  border: 1px solid #979797;
  border-radius: 2px;
  color: #979797;
  background-color: #FFFFFF;
}
.tableReview {
  width: 100%; border: 1px solid #F2F2F2; border-collapse: collapse;
}
.tableReview > tr {
  border-top: 1px solid #F2F2F2; border-bottom: 1px solid #F2F2F2;
}
.tableReview > tr > th {
  border: 1px solid #F2F2F2; 
  padding-top: 10px; padding-bottom: 10px; 
  color: #979797;
  font-style: normal;
  font-weight: normal;
  font-size: 13px;
}
.tableReview > tr > td {
  border-top: 1px solid #F2F2F2; border-bottom: 1px solid #F2F2F2;
}
</style>