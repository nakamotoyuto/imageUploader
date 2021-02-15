<template>
  <div class='box' v-show="state.upload === 'form'">
    <form class='form'>
      <div class='form__box'>
        <div>
          <h1 class='form__title'>Upload your Image</h1>
          <p class='form__titleSubTxt'>File should be Jpeg, Png,...</p>
        </div>
        <div>
          <input type='file' id='dragAndDrop' class='form__dragAndDrop'>
          <label class='form__label' for='dragAndDrop' @dragleave.prevent @dragover.prevent @drop.stop="onUpload">
            <svg-icon type='mdi' :path='state.imageSizeSelectAcutual' :size="48"/>
            <p>Drag & Drop your image here</p>
          </label>
        </div>
        <p>or</p>
        <div>
          <input type='file' id='finderSelect' class='form__finderSelect' @change="onUpload">
          <label class='form__label--finderSelect' for='finderSelect'>
            Choose a file
          </label>
          <!-- <label for='dragAndDrop'> -->
        </div>
      </div>
    </form>
  </div>
  <div class='box--uploading' v-show="state.upload === 'loading'">
    <p class='uploadingTxt'>uploading</p>
    <span class='uploadingBar'></span>
  </div>
  <div class='box--complete' v-show="state.upload === 'complete'">
    <form class='complete'>
      <div class='complete__box'>
        <div>
          <svg-icon class='complete__svg' type='mdi' :path='state.checkCircle' :size="35"/>
          <h1 class='complete__title'>Uploaded Successfully!</h1>
        </div>
        <div>
        </div>
        <div>
          <img src='https://via.placeholder.com//150x150' class='complete__img'>
          <div class="complete__copyBox">
            <input type='text' class='complete__readOnlyInput'>
            <button class='complete__copyButton'>
              Copy Link
            </button>
          </div>
          <!-- <label for='dragAndDrop'> -->
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import SvgIcon from '@jamescoyle/vue-icon'
import { mdiImageSizeSelectActual, mdiCheckCircle } from '@mdi/js';
import { reactive} from "vue";
export default {
  name: 'ImageUploader',
  components:{
    SvgIcon
  },
  setup() {
    const state = reactive({
      upload: 'form',
      imageSizeSelectAcutual: mdiImageSizeSelectActual,
      checkCircle: mdiCheckCircle
    });
    const onUpload = (event) => {
      //選択ボタンを押した時はevent.target.files　　ドラッグアンドドロップの場合はevent.dataTransfer.files
      const file = event.target.files ? event.target.files : event.dataTransfer.files;

      let formData = new FormData;

      formData.append('imagepath', file[0]);
      state.upload = 'loading'
      const param = {
        method: "POST",
        body: formData
      }
      fetch("https://example.com/receive.php", param)
        .then((res)=>{
          return( res.json() );
        })
        .then((json)=>{
          state.upload = 'complete'
          // 通信が成功した際の処理
        })
        .catch((error)=>{
          // エラー処理
          state.upload = 'form'
        });
  };
    return{state,onUpload}
  }
}
</script>
<style lang="scss" scoped>
  .box{
    margin: auto;
    padding: 36px 32px;
    background-color: #ffffff;
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
    border-radius: 12px;
    max-width:402px;
    height: 469px;
  }

  .box--uploading{
    margin: auto;
    padding: 36px 32px;
    background-color: #ffffff;
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
    border-radius: 12px;
    max-width:402px;
  }

  .form__title{
    color: #4f4f4f;
    font-size: 18px;
  }

  .form__titleSubTxt{
    margin: 15px auto 29px;
    color: #828282;
    font-size: 10px;
  }

  .form__dragAndDrop {
    display: none;
  }
  .form__finderSelect{
    display: none;
  }

  .form__label{
    padding:35px 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 218px;
    background: #F6F8FB;
    border: 1px dashed #97BEF4;
    box-sizing: border-box;
    border-radius: 12px;
    cursor: pointer;
  }

  .form__label--finderSelect{
    display: block;
    margin: 0 auto;
    width: 101px;
    line-height: 31px;
    background: #2F80ED;
    border-radius: 8px;
    font-size: 12px;
    color: #ffffff;
    cursor: pointer;
  }

  .uploadingTxt{
    text-align: left;
  }

  .uploadingBar{
    position: relative;
    display: inline-block;
    width: 100%;
    background: #F2F2F2;
    border-radius: 8px;
    height: 6px;
    overflow: hidden;
  }
  .uploadingBar:after{
    content:'';
    position: absolute;
    top: 0;
    left: -100px;
    width: 100px;
    height: 100%;
    background: #2F80ED;
    border-radius: 8px;
    animation-name: slide_bar;
    animation-duration: 2s;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
  }
  @keyframes slide_bar {
    100% {
      left: 100%;
    }
}
.box--complete{
  margin: auto;
  padding: 36px 32px;
  background-color: #ffffff;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  max-width:402px;
}
.complete__title{
  color: #4f4f4f;
  font-size: 18px;
}
.complete__svg{
  color: #219653;
}
.complete__img{
  width: 100%;
  max-height: 224.4px;
}
.complete__copyBox{
  margin-top: 25px;
  display: flex;
  justify-content: space-between;
  background: #F6F8FB;
  border: 1px solid #E0E0E0;
  box-sizing: border-box;
  border-radius: 8px;
}
.complete__copyButton{
  display: block;
  width: 101px;
  line-height: 31px;
  background: #2F80ED;
  border-radius: 8px;
  font-size: 12px;
  color: #ffffff;
  cursor: pointer;
  border: none;
}

.complete__readOnlyInput{
  width: 100%;
  border: none;
  background-color: transparent;
}
</style>
