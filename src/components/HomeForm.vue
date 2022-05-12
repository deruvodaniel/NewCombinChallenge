<template>
  <div class="form-container">
    <h2>Submit Form</h2>
    <form ref="formRef" action="submit">
      <label for="first-name">First Name
        <input
          id="first-name"
          :class="['input' ,
          { 'error-outline': $v.userData.firstName.$invalid && $v.userData.firstName.$dirty }]"
          type="text"
          placeholder="First Name"
          v-model.trim="$v.userData.firstName.$model"
        >
      </label>
        <div class="error" v-if="!$v.userData.firstName.required && $v.userData.firstName.$dirty"
        >Field is required</div>

      <label for="last-name">Last Name
        <input
          id="last-name"
          :class="['input' ,
          { 'error-outline': $v.userData.lastName.$invalid && $v.userData.lastName.$dirty }]"
          v-model="userData.lastName"
          type="text"
          placeholder="Last Name"
          v-model.trim="$v.userData.lastName.$model"
        >
      </label>
      <div class="error" v-if="!$v.userData.lastName.required && $v.userData.lastName.$dirty"
      >Field is required</div>

      <label for="adress">Adress
        <input
          id="adress"
          :class="['input' ,
          { 'error-outline': $v.userData.address.$invalid && $v.userData.address.$dirty }]"
          v-model="userData.address"
          type="text"
          placeholder="Address"
          v-model.trim="$v.userData.address.$model"
        >
      </label>
      <div class="error" v-if="!$v.userData.address.required && $v.userData.address.$dirty"
      >Field is required</div>

      <label for="ssn">Ssn
        <input
          maxlength="9"
          id="ssn"
          :class="['input' ,
          { 'error-outline': $v.userData.ssn.$invalid && $v.userData.ssn.$dirty }]"
          type="text"
          placeholder="Eg: 123456789 (No dash needed)"
          v-model.trim="$v.userData.ssn.$model"
          @change="ssnFormatter()"
        >
      </label>
      <div class="error" v-if="!$v.userData.ssn.required && $v.userData.ssn.$dirty"
      >Field is required</div>
      <div class="error" v-if="$v.userData.ssn.$invalid && $v.userData.ssn.$dirty"
      >Only 9 numbers allowed</div>

      <div class="buttons-container">
        <button @click.prevent="cleanForm()">
          Reset
        </button>
        <button
        :disabled="$v.$invalid"
        @click.prevent="getUserData()">
          Save
        </button>
      </div>

      <div v-show="showModal" class="modal-container" ref="modalRef">
        <h3>User Status</h3>
        {{this.apiResponse}}
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import { required, minLength, maxLength } from 'vuelidate/lib/validators';

export default {
  name: 'HomeForm',
  data() {
    return {
      ssnFormattedNumber: null,
      members: [],
      token: window.localStorage.getItem('token'),
      apiResponse: 'User Created Succesfully',
      showModal: false,
      userData: {
        firstName: '',
        lastName: '',
        address: '',
        ssn: '',
      },
    };
  },

  props: {
    errorOutline: {
      type: Boolean,
      default: false,
    },

    disabled: {
      type: Boolean,
      default: false,
    },
  },

  validations: {
    userData: {
      firstName: {
        required,
        minLength: minLength(1),
      },
      lastName: {
        required,
        minLength: minLength(1),
      },
      address: {
        required,
        minLength: minLength(1),
      },
      ssn: {
        required,
        minLength: minLength(9),
        maxLength: maxLength(11),
      },
    },
  },

  methods: {
    formatSSN(value) {
      const ssn = value.replace(/[^\d]/g, '');
      const ssnLength = ssn.length;
      if (ssnLength < 9) return ssn;
      if (ssnLength < 6) {
        return `${ssn.slice(0, 3)}-${ssn.slice(3)}`;
      }
      return `${ssn.slice(0, 3)}-${ssn.slice(3, 5)}-${ssn.slice(5, 9)}`;
    },

    ssnFormatter() {
      const inputField = document.getElementById('ssn');
      const formattedInputValue = this.formatSSN(inputField.value);
      inputField.value = formattedInputValue;
      this.userData.ssn = formattedInputValue;
    },

    cleanForm() {
      this.$refs.formRef.reset();
      this.showModal = false;
    },

    getUserData() {
      const post = {
        firstName: this.userData.firstName,
        lastName: this.userData.lastName,
        address: this.userData.address,
        ssn: this.userData.ssn,
      };
      const config = {
        headers: { Authorization: `Bearer ${this.token}` },
      };

      axios.post('http://localhost:8081/api/members', post, config).then((result) => {
        this.showModal = true;
        console.log(result.data);
      }).catch((error) => {
        console.log(error);
        if (error) {
          this.apiResponse = 'There was an error. Check your data (SSN duplicated)';
        }
      });
    },
  },
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

// Global styles
@import "@/scss/_mixins.scss";
@import "@/scss/_commons.scss";

.form-container {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
  flex-direction: column;
  h2 {
    margin: 30px;
    font-size: $textL;
  }
  form{
    display: flex;
    width: 60vw;
    height: 50vh;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 80px;
    background-color: rgba($color: #fff, $alpha: .6);
    border-radius: 10px;
    .error {
      color: red;
    }
    label {
      font-size: $textM;
      display: flex;
      align-items: center;
      justify-content: space-between;
      width: 100%;
      margin: 10px;
      color: $secondaryColor;
      font-weight: 600;
      span {
        font-size: $textXs;
      }
      .input {
        width: 60%;
        height: 40px;
        border: none;
        border-radius: 10px;
        background-color: $secondaryColor;
        opacity: 0.7;
        padding: 10px;
        outline: none;
        color: $mainColor;
        font-size: $textS;
        font-weight: 600;
      }
      .error-outline {
        border: 1px solid red !important;
      }
    }
    .buttons-container {
      display: flex;
      justify-content: space-around;
      align-items: center;
      padding: $padding30;
      width: 60%;
      margin-top: 30px;
      button {
        width: 150px;
        height: 50px;
        border-radius: 10px;
        border: none;
        color: $secondaryColor;
        font-size: 1.8rem;
        transition: all ease-in-out .3s;
        &:hover{
          background-color: $secondaryColor;
          color: $mainColor;
          cursor: pointer;
          transform: scale(1.1);
        }
          &:disabled{
            transform: scale(1);
            color: $mainColor;
            background-color: gray;
          }
      }
    }

    .modal-container {
      display: flex;
      width: 30vw;
      height: 20vh;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      padding: 30px;
      background-color: rgba($color: #fff, $alpha: .6);
      border-radius: 10px;
      opacity: 1;
      transition: all ease-in-out 1s;
      h3 {
        color: $secondaryColor;
      }
    }
  }

}
</style>
