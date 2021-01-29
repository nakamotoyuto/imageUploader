<template>
  <div class='box'>
    <form class='form'>
      <div class='form__box'>
        <div>
          <h1 class='form__title'>Upload your Image</h1>
          <p class='form__titleSubTxt'>File should be Jpeg, Png,...</p>
        </div>
        <div>
          <input type='file' id='dragAndDrop' class='form__dragAndDrop'>
          <label class='form__label' for='dragAndDrop'>
            <svg-icon type='mdi' :path='state.path' :size="48"/>
            <p>Drag & Drop your image here</p>
          </label>
        </div>
        <p>or</p>
        <div>
          <input type='file' id='finderSelect' class='form__finderSelect'>
          <label class='form__label--finderSelect' for='finderSelect'>
            Choose a file
          </label>
          <!-- <label for='dragAndDrop'> -->
        </div>
      </div>
    </form>
  </div>
  <div class='box'>
    <p class='uploadingTxt'>uploading</p>
  </div>
</template>

<script>
import SvgIcon from '@jamescoyle/vue-icon'
import { mdiImageSizeSelectActual } from '@mdi/js';
import { reactive} from "vue";
export default {
  name: 'ImageUploader',
  components:{
    SvgIcon
  },
  setup() {
    const state = reactive({
      path: mdiImageSizeSelectActual
    });
    return{state}
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
</style>
