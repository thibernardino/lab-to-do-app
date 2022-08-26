<template>
  <div class="ctn">
    <div class="form-side">
      <div class="header">
        <a href="/auth/#" class="header-logo">
          <img
            src="https://res.cloudinary.com/thibernar/image/upload/v1661463193/Taskapp_ov6ery.png"
            alt="Logo-TaskApp"
          />
        </a>
        <div class="header-description">
          <h3 class="header-title">Register to TaskApp</h3>
          <p class="header-subtitle">
            Organizing is a journey, not a destination.
          </p>
        </div>
      </div>

      <form @submit.prevent="signUp" class="form-sign-in">
        <div class="form">
          <div class="form-logo"></div>
          <div class="form-input">
            <label class="input-field-label">E-mail</label>
            <input
              type="email"
              class="input-field"
              placeholder="Your email for this class of thing"
              id="email"
              v-model="email"
              required
            />
          </div>
          <div class="form-input">
            <label class="input-field-label">Password</label>
            <input
              :type="confirmPasswordFieldType"
              class="input-field"
              placeholder="Type your open sesame"
              id="password"
              v-model="password"
              required
            />
          </div>
          <div class="form-input">
            <label class="input-field-label">Confirm password</label>
            <input
              :type="confirmPasswordFieldType"
              class="input-field"
              placeholder="Type it again just to be sure"
              id="confirmPassword"
              v-model="confirmPassword"
              required
            />
            <button class="button" @click.prevent="hidePassword = !hidePassword">
              Show password
            </button>
          </div>
          <button class="button" type="submit">Sign Up</button>
          <p class="signuplink">
            Have an account? Click
            <PersonalRouter
              :route="route"
              :buttonText="buttonText"
              class="sign-up-link"
            />!</p>
        </div>
      </form>

      <div v-show="errorMsg">{{ errorMsg }}</div>
    </div>

    <div class="image-side">
      <img
        src="https://res.cloudinary.com/thibernar/image/upload/v1661466874/Disen%CC%83o_sin_ti%CC%81tulo_2_svtgwf.png"
        alt="SignUpPage"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/login";
const buttonText = "here";

// Input Fields
const email = ref("");
const password = ref("");
const confirmPassword = ref("");

// Error Message
const errorMsg = ref("");

// Show hide password variable
const confirmPasswordFieldType = computed(() =>
  hidePassword.value ? "password" : "text"
);
const hidePassword = ref(true);

// Show hide confrimPassword variable

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
const signUp = async () => {
  if (password.value === confirmPassword.value) {
    try {
      // calls the user store and send the users info to backend to logIn
      await useUserStore().signUp(email.value, password.value);
      // redirects user to the homeView
      redirect.push({ path: "/auth/login" });
    } catch (error) {
      // displays error message
      errorMsg.value = error.message;
      // hides error message
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  errorMsg.value = "Password doesn't match!";
};
</script>

<style scoped>
.ctn {
  display: flex;
  justify-content: space-between;
  width: 100vw;
}

.image-side img {
  object-fit: cover;
  width: 50vw;
  height: 100vh;
}

.form-side {
  margin: auto;
  max-width: 747px;
}

.button {
  background-color: #8f8d8d;
  border: 2px solid black;
  color: white;
  padding: 10px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  cursor:pointer;
}

.header {
  text-align: center;
}

.signuplink {
  text-align: center;
}

.header img {
  width: 150px;
}

.header-subtitle {
  color: #303134;
}

.input-field-label {
  display: block;
  margin-bottom: 10px;
  font-weight: 500;
}

.input-field {
  border-radius: 3px;
  border: 1px solid #8f8d8d;
  margin-bottom: 20px;
  width: 100%;
  padding: 10px 0;
  font-family: "Roboto", sans-serif;
}

.button {
  padding: 10px 40px;
  width: 100%;
  border-radius: 5px;
}

a {
  text-decoration: none;
  color: #8f8d8d;
  transition: 0.4s;
}

.sign-up-link:hover {
  color: #393737;
}

@media only screen and (max-width: 747px) {
  .ctn {
    display: flex;
    align-items: flex-end;
    background-image: url("https://res.cloudinary.com/thibernar/image/upload/v1661466874/Disen%CC%83o_sin_ti%CC%81tulo_2_svtgwf.png");
    background-repeat: no-repeat;
    background-size: contain;
    background-position: 0% 0%;
    height: 100vh;
  }

  .image-side {
    display: none;
  }

  .form-side {
    display: flex;
    flex-direction: column;
    background-color: #fff;
    width: 100%;
  }

  .form-sign-in {
    display: flex;
    align-items: center;
    flex-direction: column;
  }

  .form-side {
    margin: 0 auto;
  }

  .header {
    display: flex;
    justify-content: center;
  }

  .header-description {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
}
</style>
