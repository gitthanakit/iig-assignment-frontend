<template>
  <div>
    <b-alert v-model="showDismissibleAlert" variant="danger" dismissible>
      Username or Password are not correct
    </b-alert>
    <b-container class="bv-example-row">
      <b-row class="vh-100" align-v="center">
        <b-card no-body class="overflow-hidden" style="width: 100%">
          <b-row no-gutters>
            <b-col md="4">
              <b-card-img
                src="https://picsum.photos/400/400/?image=20"
                alt="Image"
                class="rounded-0"
              ></b-card-img>
            </b-col>
            <b-col md="8">
              <b-card-body>
                <b-card-text>
                  <b-form @submit="onSubmit" v-if="show">
                    <b-form-group
                      id="UsernameInputgroup"
                      label="Username:"
                      label-for="UsernameInput"
                    >
                      <b-form-input
                        id="UsernameInput"
                        v-model="form.username"
                        type="text"
                        placeholder="Enter Username"
                        required
                      ></b-form-input>
                    </b-form-group>

                    <b-form-group
                      id="PasswordInputgroup"
                      label="Password:"
                      label-for="PasswordInput"
                    >
                      <b-form-input
                        id="PasswordInput"
                        v-model="form.password"
                        type="password"
                        placeholder="Enter Password"
                        required
                      ></b-form-input>
                    </b-form-group>

                    <b-button type="submit" variant="primary">Submit</b-button>
                    <nuxt-link to="/register">Register</nuxt-link>
                  </b-form>
                </b-card-text>
              </b-card-body>
            </b-col>
          </b-row>
        </b-card>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      form: {},
      show: true,
      showDismissibleAlert: false,
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      var Checklogin = false;
      axios.get("http://localhost:3000/login").then((res) => {
        for (let i = 0; i < res.data.length; i++) {
          console.log();
          if (res.data[i].username == this.form.username) {
            if (res.data[i].password == this.form.password) {
              Checklogin = true;
              break;
            }
          }
        }
        if (Checklogin) {
          localStorage.setItem("user", this.form.username);
          location.replace("/editprofile");
        } else {
          showDismissibleAlert = true;
          this.dismissCountDown = this.dismissSecs;
        }
      });
    },
  },
};
</script>
