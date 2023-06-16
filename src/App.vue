<template>
  <WidgetContent />
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import WidgetContent from './components/WidgetContent.vue';

@Options({
  components: {
    WidgetContent,
  },
})
export default class App extends Vue { }
</script>

<style lang="scss">
@use "sass:math";
@import url('./assets/style.scss');
@import url('https://fonts.googleapis.com/css2?family=Sarabun:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,400;1,500&display=swap');
$base-font-size: 16px;
$base-screen-width: 1920px;
$base-screen-font-size: $base-font-size;

#app {
  width: 100vw;
  height: 100vh;
  background-image: url('./assets/bg.png');
  background-color: black;
  background-position: left;
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  justify-content: center;
  align-items: center;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  overflow: hidden;
}

@function calculate-font-size($screen-width) {
  $base-factor: math.div($screen-width, $base-screen-width);
  $calculated-font-size: $base-factor * $base-screen-font-size;
  @return $calculated-font-size;
}

@mixin font-size($min-width) {
  $font-size: calculate-font-size($min-width);

  @media screen and (min-width: $min-width) {
    :root {
      font-size: $font-size;
    }
  }
}

@for $i from 1643px through 7680px {
  @include font-size($i);
}
@for $i from 320px through 676px
{
  $base-screen-width: 676px;
  @include font-size($i);
}
</style>
