<template>
  <div class="addDialog" v-if="in_showDialog">
    <div class="dialogDivHeader">
      <div class="icon1"><i class="fa fa-bookmark-o" aria-hidden="true"></i></div> <div class="head-caption">{{ in_caption }}</div> 
      <div class="head-close"><button class="btn btn-outline-secondary btn-sm" name="btCloseDial" v-on:click="closeDialCancel">X</button></div>
    </div>
    <div class="dialogContent">
      <table class="dialogTable">
        <colgroup>
          <col style="width: 30%">
          <col style="width: 70%">
        </colgroup>
        <tr>
          <td>Фото товара</td><td><input type="file" class="form-control" value="photoPath" @change="valChange" accept="image/*"></td>
        </tr>
        <tr v-if="in_revStatus < 2">
          <td>Наименование товара</td><td><input type="text" class="form-control" v-model="gadgetName" /></td>
        </tr>
        <tr v-if="in_revStatus < 2">
          <td colspan="2"> 
            <table class="maxtable">
            <colgroup>
              <col style="width: 30%">
              <col style="width: 35%">
              <col style="width: 35%">
            </colgroup>
            <tr>
              <td class="leftalign">Характеристики:</td><td><star-info :starballs="compStars" /></td>
              <td rowspan="4">
                <div class="preview">
                  <img v-bind:src="photoContent" />
                </div>
              </td>
            </tr>
            <tr>
              <td class="leftalign">Впечатление</td>
              <td><select class="form-control" v-model="params.impressions">
                  <option v-for="(optionBall, index1) in optionsBall" v-bind:key="index1">{{ optionBall }}</option>
                </select></td>
            </tr>
            <tr>
              <td class="leftalign">Соответствие</td>
              <td><select class="form-control" v-model="params.correspondence">
                  <option v-for="(optionBall, index2) in optionsBall" v-bind:key="index2">{{ optionBall }}</option>
                </select></td>
            </tr>
            <tr>
              <td class="leftalign">Надёжность</td>
              <td><select class="form-control" v-model="params.reliability">
                  <option v-for="(optionBall, index3) in optionsBall" v-bind:key="index3">{{ optionBall }}</option>
                </select></td>
            </tr>
            </table>
          </td>
          
        </tr>
        <tr>
          <td>Текст отзыва</td><td><textarea class="form-control" rows="3" v-model="reviewText"></textarea></td>
        </tr>
      </table>
    </div>
    <div class="dialogFooter">
      <button class="btn btn-outline-secondary btn-sm dist" name="btCloseDialOk" v-on:click="closeDialOk">OK</button>
      <button class="btn btn-outline-secondary btn-sm dist" name="btCloseDialCancel" v-on:click="closeDialCancel">Отмена</button>
    </div>
  </div>
</template>

<script>
import starInfo from './starInfo.vue';

export default {
  name: 'addDialog',
  components: {
    'star-info':starInfo
  },
  props: {
    showdialog: Boolean,
    revStatus: Number,
    dialCaption: String,
  },
  data() {
    return {
      in_showDialog: false,
      in_revStatus: this.revStatus, //0 - активный отзыв, 1 - неактивный отзыв, 2 - доп отзыв (текст строка и фото)
      in_caption: this.dialCaption,
      optionsBall:[1,2,3,4,5],
      photoPath:'',
      photoContent: '',
      gadgetName: '',
      reviewText: '',
      params: {stars: 5, impressions: 5, correspondence: 5, reliability: 5},
      advancedString: ''
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
    showdialog: function(newValue) {
      this.in_showDialog = newValue;
    },
    revStatus:function(newValue) {
      this.in_revStatus = newValue;
    },
    dialCaption:function(newValue) {
      this.in_caption = newValue;
    }
  },
  computed: {
    compStars() {
      this.calcStars();
      return this.params.stars;
    }
  },
  methods: {
    calcStars() {
      let srArifm = (this.params.impressions*1 + this.params.correspondence*1 + this.params.reliability*1)/3;
      this.params.stars = Math.round(srArifm * 10) / 10;
    },
    closeDialOk() {
      this.in_showDialog = false;
      this.calcStars();
      
      let resultData = {
        closeCode: 0,
        photoPath: this.photoPath,
        gadgetName: this.gadgetName,
        reviewText: this.reviewText,
        stars: this.params.stars*1, impressions: this.params.impressions*1, 
        correspondence: this.params.correspondence*1, reliability: this.params.reliability*1,
        photoContent: this.photoContent,
        revStatus: this.in_revStatus,
        revStrState: 0,//Статус отзыва (0 - опубликован, 1 - на модерации, 2 - отказ в публикации)
        rowIndex: -1,//Индекс строки компонента (индекс в массиве)
        parentIndex: -1,//Индекс родителя (-1 для основного отзыва)
        advFlag: false,//Флаг "далее доп отзывы к основному" - ставится у родителя, если есть допы. У допов = false
        showAdv: false,//Флаг показа доп отзывов текущего компонента
      };
      this.$emit('closeDial',resultData);
    },
    closeDialCancel() {
      this.in_showDialog = false;
      let resultData = {
        closeCode: 1
      };
      this.$emit('closeDial',resultData);
    },
    valChange(val) {
      let fileName = val.target.files[0];
      //let fileType = fileName.type;//'image/jpeg', 'image/png'
      
      let reader = new FileReader();
      reader.onload = this.onFileLoad;
      reader.readAsDataURL(fileName);
      
      //console.log('fileFPath:',fileFPath,'fileName:',fileName);
    },
    onFileLoad(e) {
      this.photoContent = e.target.result;
    },
  }
}
</script>

<style scoped>
  .addDialog {
    display: block; 
    background-color: white;
    border: 2px solid gray;
    border-radius: 10px;
    position: absolute; 
    left: 250px; top: 50px;
    width: 70%;
    height: 80%;
    
  }
  .dialogDivHeader {
    display: flex; flex-direction: row; flex-wrap: nowrap; 
    justify-content: space-between;
    border-bottom: 1px solid gray;
    box-sizing: border-box;
    width: 100%; height: 10%;
    padding: 5px 5px 5px 5px;
  }
  .dialogContent {
    display: block;
    box-sizing: border-box;
    width: 100%; height: 80%;
    margin-top: -5px;
  }
  .dialogFooter {
    display: flex; flex-direction: row; flex-wrap: nowrap; 
    border-top: 1px solid gray;
    justify-content: center;
    box-sizing: border-box;
    width: 100%; height: 10%;
    padding: 5px 5px 5px 5px;
  }
  .dialogTable {
    margin: 10px 10px 10px 10px;
    width: 95%;
    box-sizing: border-box;
  }
  .maxtable {
    width: 100%;
    box-sizing: border-box;
  }
  .preview {
    display: block;
    width: 400px; height: 180px;
    box-sizing: border-box;
    overflow-x: scroll;
    overflow-y: scroll;
  }
  .leftalign {
    padding-left: 30px;
  }
  .dist {
    margin-left: 5px; margin-right: 5px;
    margin-top: 5px; margin-bottom: 5px;
  }
</style>