<template>
    <div id="incomingCall">
        <img src="../assets/yuying-icon.png" alt="">
        <img src="../assets/shipin-icon.png" alt="">
        <span>来自zzh的呼入</span>
        <img src="../assets/close-icon.png" alt="">
    </div> 
</template>
<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";

@Component({
  props: {
    // imgObj: Object
  }
})
export default class IncomingCall extends Vue {
  created() {}

  mounted() {}
}
</script>

<style lang="scss">
#incomingCall {
  width: 100%;
  height: 100%;
  img {
    width: 26px;
    height: 26px;
    margin-right: 10px;
    margin-top: 5px;
    &:nth-child(4) {
      margin-right: 0;
    }
  }
  span {
    display: inline-block;
    height: 26px;
    line-height: 26px;
    position: relative;
    bottom: 8px;
  }
}
</style>


