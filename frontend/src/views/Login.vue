<template>
  <main id="app">
    <h1>Welcome to Groupomania</h1>
    <form class="signin">
      <div class="form">
        <label for="email">Email address</label> <br />
        <input type="email" class="form-control" v-model="email" id="email" aria-required="true" placeholder="example@groupomania.com" required /><br />
        <span class="error" v-if="!$v.email.required && $v.email.$dirty">Please provide a valid email address.</span>
      </div>
      <div class="form">
        <label for="password">Password</label><br />
        <input type="password" class="form-control" v-model="password" id="password" aria-required="true" required /><br />
        <span class="error" v-if="!$v.password.required && $v.password.$dirty">Please choose a password with at least 8 characters, 1 uppercase letter, 1 lowercase letter, and no spaces.</span>
        <span class="error" v-if="!$v.password.valid && !$v.password.minLength">Please choose a password with at least 8 characters, 1 uppercase letter, 1 lowercase letter, and no spaces.</span>
      </div>
      <button type="submit" class="btn signup" v-on:click="loginUser()">Login</button><br />
      <span id="notfound" class="error" v-if="!$v.email.valid && $v.email.$dirty">Cannot find user. Please check your login informations. </span>
    </form>
    <div class="encourage">
      <p class="encouragement encourage-signup">Not a member yet ? Please signup !</p>
      <router-link class="signup" to="/Signup"><button class="btn signup">Signup</button></router-link>
    </div>
    <Footer />
  </main>
</template>

<script>
import Footer from "@/components/Footer.vue";
import axios from "axios";
import { required, email, minLength, maxLength } from "vuelidate/lib/validators";

export default {
  name: "Login",
  components: {
    Footer,
  },
  data() {
    return {
      email: "",
      password: "",
    };
  },
  validations: {
    email: {
      required,
      email,
    },
    password: {
      required,
      valid: function (value) {
        const containsUppercase = /[A-Z]/.test(value);
        const containsLowercase = /[a-z]/.test(value);

        return containsUppercase && containsLowercase;
      },
      minLength: minLength(8),
      maxLength: maxLength(20),
    },
  },
  methods: {
    loginUser() {
      this.submited = true;
      this.$v.$touch();
      console.log("login");
      if (!this.$v.$invalid) {
        const email = document.querySelector("#email").value;
        const password = document.querySelector("#password").value;
        const user = {
          email: email,
          password: password,
        };

        axios
          .post(this.$localhost + "api/auth/login", user, {
            header: {
              "Content-Type": "application/json",
            },
          })
          .then((res) => {
            localStorage.setItem("token", res.data.token);
            this.$router.push('/home');
          })
          .catch((error) => {
            console.log(error);
            document.getElementById("notfound").innerHTML = "Cannot find user. Please check your login informations. ";
          });
      } else {
        document.getElementById("notfound").innerHTML = "Cannot find user. Please check your login informations. ";
      }
    },
  },
};
</script>

<style scoped>
#app {
  text-align: center;
}
h1 {
  padding-top:30px;
}
::placeholder {
  color:rgba(245, 245, 245, 0.623);
}
.signin {
  width: 50%;
  margin: 70px auto auto auto;
}
.signup {
  margin-bottom: 40px;
}
.form {
  margin: 10px 0 20px 0;
}
.error {
  color: whitesmoke;
  font-size: 20px;
  font-style:bold;
}
@media (max-width: 1024px) {
  .signin {
    width: 100%;
    margin: 0;
  }
  h1 {
    font-size: 20px !important;
  }
  .form-control {
    width:90%;
  }
}
</style>
