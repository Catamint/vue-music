<template>
  <HeaderLayout  v-show="!utils.play_component.showing"/>
    <div class="container"  v-show="!utils.play_component.showing">
      <SidebarLayout />
      <div class="col-8">
        <div class="model">
          <div class="col">
              <!-- <keep-alive include="/"> -->
              <router-view v-if="isRefreshFlag" v-slot="{ Component }">
                <transition name="slide-up">
                <component :is="Component" />
                </transition>
              </router-view>
              <!-- </keep-alive> -->
          </div>
        </div>
      </div>
    </div>
  <Transition name="slide">
    <Play v-show="utils.play_component.showing" />
  </Transition>
  <FooterLayout />

</template>

<script>

import FooterLayout from './layout/footer/FooterLayout.vue';
import HeaderLayout from './layout/header/HeaderLayout.vue';
import SidebarLayout from "./layout/sidebar/SidebarLayout.vue";
import Home from '../views/home/Home.vue';
import Play from '@/views/play/Play.vue';
import { utils } from '@/stores/utils';
import { NScrollbar } from 'naive-ui';
import {ref, nextTick, provide} from "vue";

export default {
  name: 'IndexView',
  components: {
    HeaderLayout,
    FooterLayout,
    SidebarLayout,
    Home,
    Play,
    NScrollbar
  },
  data() {
    return {
      utils,
      isRefreshFlag: true
    }
  },
  methods: {
    reloadPage() {
      this.isRefreshFlag = false
      nextTick(() => {
        this.isRefreshFlag = true
      })
    }
  },
  provide(){
    return {
      reloadPage: this.reloadPage
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex;
  justify-content: space-between;
  flex: 1 1 auto;
}

.col-8 {
  display: flex;
  /* padding: 1%; */
  /* padding-left: 0; */
  width: 100%;
}

.model {
  position: relative;
  top: 0;
  bottom: 0;
  width: 100%;
}

.col {
  display: block;
  box-sizing: border-box;
  position: absolute;
  /* background-color: rgba(255, 255, 255, 0.352); */
  padding-left: 20px;
  padding-right: 20px;
  /* border-radius: 20px; */
  border-top-left-radius: 20px;
  width: 100%;
  top: 0;
  bottom: 0;
  /* overflow: auto; */
}

@keyframes slideDown {
  0% {
    transform: translateY(0);
    opacity: 1;
  }
  100% {
    transform: translateY(100%);
    opacity: 0;
  }
}
@keyframes slideup {
  0% {
    transform: translateY(100%);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}
.slide-enter-active {
  animation: slideup 0.5s;
}
.slide-leave-active {
  animation: slideDown 0.5s;
}

.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.25s ease-out;
}
.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}
.slide-up-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}
</style>
