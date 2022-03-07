<template>
  <div>
    <b-container class="bv-example-row">
      <b-row class="vh-100" align-v="center">
        <b-card no-body class="overflow-hidden" style="width: 100%">
          <b-row no-gutters>
            <b-col md="4">
              <b-img
                src="https://images.unsplash.com/photo-1503023345310-bd7c1de61c7d?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwcm9maWxlLXBhZ2V8N3x8fGVufDB8fHx8&w=1000&q=80"
                fluid
                alt="Responsive image"
                style="height: 100%"
              ></b-img>
            </b-col>
            <b-col md="8">
              <b-card-body title="Edit Profile">
                <b-card-text>
                  <b-form @submit="onSubmit" @reset="onReset" v-if="show">
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
                        disabled
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

                    <b-form-group
                      id="FullnameInputgroup"
                      label="Full Name:"
                      label-for="FullnameInput"
                    >
                      <b-form-input
                        id="FullnameInput"
                        v-model="form.fullname"
                        type="text"
                        placeholder="Enter Fullname"
                        required
                      ></b-form-input>
                    </b-form-group>

                    <b-form-group
                      id="LastnameInputgroup"
                      label="Lastname:"
                      label-for="LastnameInput"
                    >
                      <b-form-input
                        id="LastnameInput"
                        v-model="form.lastname"
                        type="text"
                        placeholder="Enter Lastname"
                        required
                      ></b-form-input>
                    </b-form-group>

                    <b-button type="submit" variant="primary">Submit</b-button>
                    <b-button type="reset" variant="danger">Cancle</b-button>
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
    };
  },
  async mounted() {
    this.getUserData();
  },
  methods: {
    async getUserData() {
      const username = localStorage.getItem("user");
      const findUser = await axios
        .get(`http://localhost:3000/login/${username}`)
        .then((res) => res.data);
      if (findUser) {
        this.form = findUser;
      }
    },
    async onSubmit(event) {
      event.preventDefault();
      let data = { ...this.form };
      //delete data.profilePath;
      var count = true;
      let regex = /\w/;
      let regexpassword = /(?=.*\w)(\w)[\da-z]/;

      if (data.username.length < 13) {
        if (regex.test(data.username)) {
          if (data.password.length != 0 && data.password.length > 6) {
            if (regexpassword.test(data.password)) {
              const payload = {
                fullname: this.form.fullname,
                lastname: this.form.lastname,
                username: this.form.username,
                password: this.form.password,
              };
              await axios
                .put(
                  `http://localhost:3000/login/${this.form.username}`,
                  payload
                )
                .then((res) => {
                  console.log("response", res);
                });
            } else {
              alert(JSON.stringify(" password must mix alphabet with number "));
            }
          } else {
            alert(JSON.stringify(" password must has more than 6 alphabet "));
          }
        } else {
          alert(
            JSON.stringify(" Username should be alphabet and number only ")
          );
        }
      } else {
        alert(JSON.stringify(" Username must has only 12 charactor "));
      }
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      location.replace("/");
    },
  },
};
</script>
