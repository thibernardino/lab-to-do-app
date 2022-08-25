<template>
  <div class="container">
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

      <p v-show="errorMsg" class="error-msg">{{ errorMsg }}</p>

      <form @submit.prevent="signIn" class="form-sign-in">
        <div class="form">
          <div class="form-logo"></div>
          <div class="form-input">
            <label class="input-field-label">E-mail</label>
            <input
              type="email"
              class="input-field"
              placeholder="example@gmail.com"
              id="email"
              v-model="email"
              required
            />
          </div>
          <div class="form-input">
            <label class="input-field-label">Password</label>
            <input
              :type="passwordFieldType"
              class="input-field"
              placeholder="**********"
              id="password"
              v-model="password"
              required
            />
            <button class="button" @click.prevent="hidePassword = !hidePassword">
              Show password
            </button>
          </div>
          <button class="button">Sign In</button>
          <p>
            Don't have an account? Click
            <PersonalRouter
              :route="route"
              :buttonText="buttonText"
              class="sign-up-link"
            />!</p>
        </div>
      </form>
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
const route = "/auth/sign-up";
const buttonText = "here";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

//Show hide password variables
const passwordFieldType = computed(() =>
  hidePassword.value ? "password" : "text"
);
const hidePassword = ref(true);

const changePasswordFieldValue = () => {
  hidePassword.value = !hidePassword.value;
};

// Router to push user once SignedIn to the HomeView
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
    // calls the user store and send the users info to backend to logIn
    await useUserStore().signIn(email.value, password.value);
    // redirects user to the homeView
    redirect.push({ path: "/" });
  } catch (error) {
    // displays error message
    errorMsg.value = `Error: ${error.message}`;
    // hides error message
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-between;
  width: 100vw;
}

.image-side img {
  object-fit: cover;
  width: 50vw;
  height: 100vh;
}

.form-input {
  position: relative;
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
  border: 1px solid #195abd;
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

.error-msg {
  color: #8f8d8d;
}

.fas {
  color: #303134;
  position: absolute;
  left: 90%;
  bottom: 30%;
  opacity: 0.6;
  transform: translate(-50%, -50%);
}

@media only screen and (max-width: 747px) {
  .container {
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

  .fas {
    display: none;
  }
}
</style>
