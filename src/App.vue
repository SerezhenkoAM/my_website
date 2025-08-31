<template>
  <div class="wrapp">
    <transition name="fade">
      <div class="load" v-if="!show_flag">
        <Spinner :show="!show_flag"/>
      </div>
    </transition>
    <Card class="card"  @mounted="is_mounted" v-show="show_flag"/>
    <transition>
        <img src="@/assets/background-4.jpg" alt="Background" class="bg" v-show="show_flag" @load="is_mounted(true)"/>
    </transition>
  </div>
  <custom-cursor></custom-cursor>
</template>

<script>
import Spinner from './components/Spinner.vue'
import Card from './components/Card.vue'
import CustomCursor from './components/CustomCursor.vue'
export default {
  data() {
    return {
      activate_logo: false,
      a: false,
      show_flag: false,
    }
  },
  methods: {
    is_mounted(bck_img_val) {
      if (bck_img_val == true) {
        setTimeout(() => {
          this.show_flag = true
        },1000)
      }
    }
  },
  components: {
    Spinner,
    Card,
    CustomCursor
  }
  
}
</script>

<style scoped lang="less">
@import url('https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&family=Roboto:ital,wght@0,100..900;1,100..900&display=swap');
:root {
  font-style: 'Roboto', 'Lato', Times New Roman;
  transition: all .5s ease-in-out;
}

.bg {
  z-index: -1;
  height: 100vh;
  width: 100vw;
  position: absolute;
  top: 0;
  object-fit: cover;
  position: fixed;
}

.v-enter-active,
.v-leave-active {
  transition: opacity .1s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}


.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

// На айфоне mini обрезаются верхние и нижние края
.card {
  margin: 5vh auto;
}
.logo_wrapp {
  background: rgba(255,255,255,0.1);
  border-radius: 25px;
  backdrop-filter: blur(4px);
  border: 2px solid rgba(255,255,255,0.2);
  padding: 10px;
  position: absolute;
  top: 2rem;
  left: 2rem;
}



* {
  cursor: none !important;
  -webkit-touch-callout: none;
-webkit-user-select: none;
-khtml-user-select: none;
-moz-user-select: none;
-ms-user-select: none;
user-select: none;
}


.load {
  width: 100vw;
  height: 100vh;
  z-index: 999;
  background-color: white;
  position: absolute;
  top: 0;
}

</style>