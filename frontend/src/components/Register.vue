<template>
  <h3 class="margin-b-30 title">Sign up</h3>
  <!-- Register form -->
  <div class="form">
    <input class="form-input" type="name" placeholder="Full name" name="name" v-model="input.name" required>
    <input class="form-input" type="email" placeholder="Email" name="email" v-model="input.email" required>
    <div class="inline-block">
      <input class="form-input margin-b-15 password-input" v-if="showPassword" type="text" placeholder="Password" name="password"
        v-model="input.password" required>
      <input class="form-input margin-b-15 password-input" v-else type="password" placeholder="Password" name="password"
        v-model="input.password" required>
      <!-- Hide show password -->
      <div class="control">
        <button @click="toggleShow" class="button-icon">
          <img :src="showPassword ? require('../assets/Invisible.png') : require('../assets/Visible.png')" class="icon">
        </button>
      </div>
    </div>
    <!-- Register button -->
    <button @click="Register()" class="margin-b-15 button-login" type="submit">Sign up</button>
  </div>
</template>

<script>
  import register from "../modules/register"

  export default {
    name: 'RegisterUser',

    setup() {
      const { input, Register } = register();
      return { input, Register };
    },

    data() {
      return {
        showPassword: false,
        password: null,
      };
    },
    computed: {
      buttonLabel() {
        return this.showPassword ? "Hide" : "Show";
      },
    },
    methods: {
      toggleShow() {
        this.showPassword = !this.showPassword;
      },
    },
  }
</script>

<style scoped lang="scss">
  .form {
    display: flex;
    flex-direction: column;
    width: 400px;
  }

  .form-input {
    height: 40px;
    width: 100%;
    margin-bottom: 15px;
    text-align: center;
    border: none;
    background-color: var(--white);
  }

  .password-input {
    margin-right: -40px;
  }

  .button-login {
    margin: 15px auto 30px auto;
    width: 400px;
    height: 40px;
    text-align: center;
    color:#232323;
    background-color: var(--light-green);
    border: none;
    cursor: pointer;
  }

  .button-login:hover {
    font-weight: bold;
    transition: 0.5s;
  }

  .title {
    color: var(--white);
    text-align: center;
  }

  .inline-block {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: space-between;
    margin: 0;
  }

  .control {
    width: 30px;
  }

  .icon {
    margin: 0;
    border: none;
    cursor: pointer;
  }

  .button-icon {
    border: none;
    padding: 0;
    height: 40px;
  }
</style>