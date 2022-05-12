<template>
  <div class="table-container">
    <h1>Table Members</h1>
    <TableMembers />
  </div>
</template>

<script>
import TableMembers from '@/components/TableMembers.vue';

export default {
  name: 'TableView',
  components: {
    TableMembers,
  },
};

</script>

<style lang="scss">
// Global styles
@import "@/scss/_mixins.scss";
@import "@/scss/_commons.scss";

.table-container{
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  h1 {
    margin: 30px;
    font-size: $textL;
  }
}
</style>
