<template>
  <div>
    <div>
      <form action="#" @submit.prevent="login">
        <div>
          <h5>Username</h5>
          <b-form-input
            v-model="form.name"
            id="input-default"
            placeholder="Enter Username"
            type="text"
          ></b-form-input>
        </div>
        <div>
          <h5>Password</h5>
          <b-form-input
            v-model="form.password"
            placeholder="Enter Password"
            type="password"
          ></b-form-input>
        </div>
        <div v-if="messageError">{{ messageError }}</div>
        <div v-if="errors">{{ errors }}</div>
        <b-button type="submit" variant="success">Login</b-button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messageError: null,
      errors: null,
      form: {
        name: "",
        password: "",
      },
    };
  },
  methods: {
    async login() {
      try {
        await this.$auth.loginWith("local", {
          data: this.form,
        });
        this.$router.push({ path: "index" });
      } catch (error) {
        this.messageError = error;
      }
    },
  },
};
</script>

<style>
</style>