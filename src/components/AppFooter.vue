<template>
  <div class="footer-container">
    <h5>
      Copyright
    </h5>

    <h5>
      all rights reserved
    </h5>
  </div>
</template>

<script>
export default {
  name: 'AppFooter',
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
// Global styles
@import "@/scss/_mixins.scss";
@import "@/scss/_commons.scss";

.footer-container {
  position: absolute;
  bottom: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  align-content: center;
  width: 100%;
  min-width: 100;
  background-color: $mainColor;
  padding: 10px;
  box-sizing: border-box;
}
</style>
