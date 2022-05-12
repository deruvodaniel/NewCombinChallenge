<template>
  <div class="users">
    <HomeForm />
  </div>
</template>

<script>
// @ is an alias to /src
import HomeForm from '@/components/HomeForm.vue';

export default {
  name: 'UserView',
  components: {
    HomeForm,
  },
};
</script>

<style lang="scss">
// Global styles
@import "@/scss/_mixins.scss";
@import "@/scss/_commons.scss";

.users{
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
</style>
