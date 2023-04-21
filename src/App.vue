<template>
  <div class="wrapper">
    <header-block />
    <router-view></router-view>
    <footer-block />
  </div>
</template>

<script>
import HeaderBlock from './components/HeaderBlock.vue'
import FooterBlock from './components/FooterBlock.vue'
export default {
  components: {
    HeaderBlock,
    FooterBlock
  }
}
</script>

<style></style>
