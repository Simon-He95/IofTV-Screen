<template>
  <!-- <div id="index" ref="appRef" class="index_home" :class="{ pageisScale: isScale }"> -->
  <ScaleScreen :width="1920" :height="1080" class="scale-wrap" :selfAdaption="$store.state.setting.isScale">
    <div class="bg">
      <dv-loading v-if="loading">Loading...</dv-loading>
      <div v-else class="host-body">
        <!-- 头部 s -->
        <div class="d-flex jc-center title_wrap">
          <div class="zuojuxing"></div>
          <div class="youjuxing"></div>
          <div class="guang"></div>
          <div class="d-flex jc-center">
            <div class="title">
              <span class="title-text">互联网设备可视化平台</span>
            </div>
          </div>
          <div class="timers"> {{ dateYear }} {{ dateWeek }} {{ dateDay }} <i class="blq-icon-shezhi02"
              style="margin-left: 10px" @click="showSetting"></i>
          </div>
        </div>
        <!-- 头部 e-->
        <!-- 内容  s-->
        <Main></Main>
         <!-- <button class="bg-red-100">button</button> -->
        <!-- 内容 e -->
      </div>
    </div>
    <Setting ref="setting" />
  </ScaleScreen>
  <!-- </div> -->
</template>
<script setup>
  import { formatTime } from "../utils/index.js";
  import Setting from "./setting.vue";
  import ScaleScreen from "@/components/scale-screen/scale-screen.vue";
  import { ref, onBeforeUnmount } from 'vue'
  import Main from './indexs/index.vue'
  let timer = null
  const dateDay = ref(null)
  const dateYear = ref(null)
  const dateWeek = ref(null)
  const loading = ref(true)
  const weekday = ["周日", "周一", "周二", "周三", "周四", "周五", "周六"]
  const setting = ref(null)
  function timeFn() {
    timer = setInterval(() => {
      dateDay.value = formatTime(new Date(), "HH: mm: ss");
      dateYear.value = formatTime(new Date(), "yyyy-MM-dd");
      dateWeek.value = weekday[new Date().getDay()];
    }, 1000);
  }
  timeFn()
  cancelLoading()

  function cancelLoading() {
    let timer = setTimeout(() => {
      loading.value = false;
      clearTimeout(timer);
    }, 500);
  }

  function showSetting() {
    setting.value.init();
  }

  onBeforeUnmount(() => {
    clearInterval(timer)
  })
</script>
<style lang="scss">
  @import "./home.scss";
</style>
