<template>
  <div class="table">
    <table>
      <thead>
        <tr>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Address</th>
          <th>SSN</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="member in members"
        :key="member.ssn"
      >
          <td><div>{{ member.firstName }}</div></td>
          <td><div>{{ member.lastName }}</div></td>
          <td><div>{{ member.address }}</div></td>
          <td><div>{{ member.ssn }}</div></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'TableMembers',

  data() {
    return {
      members: [],
      timer: null,
    };
  },

  mounted() {
    // Refreshing Page after 2min
    this.timer = setInterval(() => {
      window.location.reload();
    }, 120000);
  },

  beforeDestroy() {
    clearInterval(this.timer);
  },

  created() {
    const token = window.localStorage.getItem('token');
    const config = {
      headers: { Authorization: `Bearer ${token}` },
    };

    axios.get(
      'http://localhost:8081/api/members',
      config,
    ).then((result) => {
      this.members = result.data;
    }).catch(console.log);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
// Global styles
@import "@/scss/_mixins.scss";
@import "@/scss/_commons.scss";

.table {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
  flex-direction: column;
  table {
    display: table;
    width: 80vw;
    min-height: 60vh;
    max-height: fit-content;
    align-items: center;
    flex-direction: column;
    padding: $padding30;
    background-color: rgba($color: #fff, $alpha: .6);
    border-radius: 10px;
    th {
      height: 40px;
      background-color: $secondaryColor;
    }
    td {
      width: 25%;
      background-color: #fff;
      font-size: $textS;
      font-weight: 600;
    }
    :last-child {
      border-radius: 0 10px 10px 0;
    }
    :first-child {
      border-radius: 10px 0 0 10px;
    }
  }
}
</style>
