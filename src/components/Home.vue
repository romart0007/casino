<template>
  <b-container fluid class="bg-holder pb-5 pr-0 pl-0" ref="bgHolder" :style="styleBgHolder">
    <b-container class="hero pb-2" ref="hero" :style="heroHeight">
      <b-row class="h-100 mb-3">
        <b-col class="text-white pt-5" cols="12" sm="6">
          <div class="promo-container">
            <p class="bg-info">Exclusive offers from / logo</p>
            <h4>Get Your</h4>
            <h1>100% up to &#163; 400</h1>
            <h2>+200 Free Spins</h2>
          </div>
        </b-col>
        <b-col class="text-white pt-5" cols="12" sm="6">
          <div class="form-container mx-auto" ref="myForm" :class="fadeClass">
            <FormStepOne v-on:goToStep="goToStepMain" v-if="currentStep == 1"/>
            <FormStepTwo v-on:goToStep="goToStepMain" v-if="currentStep == 2"/>
            <FormStepThree v-on:goToStep="goToStepMain" v-if="currentStep == 3"/>
            <FormStepFour v-on:goToStep="goToStepMain" v-if="currentStep == 4"/>
          </div>
        </b-col>
      </b-row>
      <b-row class="legal-text pt-2">
        <b-col>
          <p>&#43; Terms and conditions apply</p>
        </b-col>
        <b-col>
          <p>This website is licensed to offer online casino.</p>
        </b-col>
      </b-row>
    </b-container>
  </b-container>
</template>
<script>
import FormStepOne from "./forms/FormStepOne";
import FormStepTwo from "./forms/FormStepTwo";
import FormStepThree from "./forms/FormStepThree";
import FormStepFour from "./forms/FormStepFour";
export default {
  data() {
    return {
      currentStep: 1,
      window,
      fadeClass: "fade-in",
      styleBgHolder: {
        height: ""
      },
      heroHeight: {
        height: ""
      }
    };
  },
  methods: {
    goToStepMain(value) {
      this.currentStep = value;
    },
    handleResize() {
      this.window.width = window.innerWidth;
      if (this.window.width <= 767) {
        this.fadeClass = "slide-in";
      } else {
        this.fadeClass = "fade-in";
      }
    },
    dynamicHeight() {
      const bgHolder = this.$refs.bgHolder.clientHeight;
      let hero = parseInt(this.$refs.hero.clientHeight + 100);

      if (
        bgHolder < hero &&
        window.matchMedia("(orientation: landscape)").matches
      ) {
        this.styleBgHolder.height = "auto";
        this.heroHeight.height = "auto";
        console.log(true);
      } else {
        console.log(false);
        this.styleBgHolder.height = "100vh";
        this.heroHeight.height = "";
      }
    }
  },
  components: { FormStepOne, FormStepTwo, FormStepThree, FormStepFour },
  mounted() {
    window.addEventListener("resize", debounce(this.handleResize), 250);
    window.addEventListener("resize", debounce(this.dynamicHeight), 250);
    this.handleResize();
    this.dynamicHeight();

    function debounce(func, wait, immediate) {
      var timeout;
      return function() {
        var context = this,
          args = arguments;
        var later = function() {
          timeout = null;
          if (!immediate) func.apply(context, args);
        };
        var callNow = immediate && !timeout;
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
        if (callNow) func.apply(context, args);
      };
    }
  }
};
</script>

<style lang="scss">
.bg-holder {
  background-image: url("/src/assets/casino-hero.jpg");
  background-position: center center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 100vh;
  padding-top: 5em;
  position: relative;

  &::before {
    background: rgba(black, 0.3);
    display: block;
    content: "";
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    position: absolute;
  }
}

.bg-info:not(.navbar) {
  background-color: rgba(23, 162, 184, 0.2) !important;
  padding: 10px;
}

.form-container {
  box-shadow: 0 0 20px #000;
  background: #fff;
  padding: 5%;
  border-radius: 5px;
  position: relative;
  z-index: 2;
}

.input-group {
  margin-bottom: 10px;
}

// .input-group-text {
//   background-color: #fff;
// }

.form-control {
  &:focus,
  &:active,
  &:hover {
    box-shadow: none;
  }
}

.legal-text {
  position: absolute;
  bottom: 0;
  left: 0;
  display: flex;
  justify-content: space-between;
  width: 100%;

  p {
    color: rgba(255, 255, 255, 0.7);
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translate(0, 10px);
  opacity: 0;
}

@media screen and (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
  height: 130vh;
}

@media only screen and (min-width: 1200px) {
  .promo-container {
    max-width: 80%;
  }
  .form-container {
    max-width: 450px;
  }
}

@media screen and (max-width: 767px) {
  .container {
    max-width: 100%;
  }
}

@media screen and (max-width: 675px) {
  .bg-holder {
    height: 100% !important;
    padding-top: 4em;

    .container {
      max-width: 80%;
    }

    .col-12 {
      -ms-flex: 0 0 100%;
      flex: 0 0 100%;
      max-width: 100%;
    }

    .legal-text {
      display: none;
    }
  }
}

@media screen and (max-width: 576px) {
  .bg-holder {
    background: transparent;
    .container {
      max-width: 95%;
    }
  }
}

@media only screen and (min-width: 675px) and (max-width: 991px) {
  .promo-container p {
    display: none;
  }
}
</style>