<template>
  <link rel=”stylesheet”
  href=”https://cdn.jsdelivr.net/npm/bootstrap-icons@1.3.0/font/bootstrap-icons.css”
  /> <link rel=”stylesheet”
  href=”https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css”
  integrity=”sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T”
  crossorigin=”anonymous”>
  <transition name="slide-fade">
    <div v-if="showP" class="contenedor-todo">
      <div class="container">
        <div class="header">
          <div class="header-description">
            <h3 class="header-title">Log In</h3>
            <lottie-player
              class="chicoslogin"
              src="https://lottie.host/656cd3c8-2d73-4a7d-9327-fe38ecb3aaa8/ymxrZpcczu.json"
              background="transparent"
              speed="1"
              style="width: 120px; height: 120px"
              loop
              autoplay
            ></lottie-player>

            <p class="header-subtitle">Start organizing your tasks!</p>
          </div>
        </div>

        <form @submit.prevent="chargeSignF" class="form-sign-in">
          <div class="form placeholders">
            <div class="form-input">
              <label class="input-field-label">E-mail</label>
              <input
                type="email"
                class="email-input"
                placeholder="example@gmail.com"
                id="email"
                v-model="email"
                required
              />
            </div>

            <div class="form-input">
              <label class="input-field-label">Password</label>
              <input
                type="password"
                class="email-input"
                placeholder="**********"
                id="password"
                v-model="password"
                required
                name="password"
              />
            </div>

            <div id="container">
              <button class="stripe-button" v-if="showHidebutton">
                Sign
                <svg
                  class="HoverArrow"
                  width="12"
                  height="12"
                  aria-hidden="true"
                >
                  <g fill-rule="evenodd">
                    <path class="HoverArrow__linePath" d="M0 6h8" />
                    <path class="HoverArrow__tipPath" d="m1 1 5 5-5 5" />
                  </g>
                </svg>
              </button>

              <lottie-player
                v-if="chargeSign"
                src="https://lottie.host/92e6d512-851e-4397-a2b0-5c155e722596/CkPjTIGKMD.json"
                background="transparent"
                speed="1"
                style="width: 200px; height: 200px"
                loop
                autoplay
              ></lottie-player>
            </div>

            <div class="haveAccount">
              Don't have an account?
              <PersonalRouter
                :route="route"
                :buttonText="buttonText"
                class="sign-up-link"
              />
            </div>
          </div>
        </form>
        <div v-show="errorMsg">{{ errorMsg }}</div>
      </div>
      <!-- <div class="imagen-lado-form"></div> -->
      <section class="min-height-account-login"></section>
    </div>
  </transition>
  <div>
    <FooterVue />
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";
import FooterVue from "../components/footer.vue";

// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";
// Arrow function to Signin user to supaBase
const email = ref("");
const password = ref("");
const errorMsg = ref("");
const redirect = useRouter();
async function signIn() {
  await useUserStore().signIn(email.value, password.value);
  redirect.push({ path: "/" });
}
/*
const signIn = async () => {
  try {
    const { error } = await supabase.auth.signIn({
      email: email.value,
      password: password.value,
    });
    useRouter().push({ path: "/" });
    if (error) throw error;
  } catch (error) {
    alert(error.error_description || error.message);
  }
};
*/

const showP = ref(false);
const loadComponent = () => {
  setTimeout(() => {
    showP.value = true;
  }, 50);
  // console.log("hola");
};
loadComponent();

const showHidebutton = ref(true);
const chargeSign = ref(false);
const chargeSignF = () => {
  chargeSign.value = !chargeSign.value;
  showHidebutton.value = !showHidebutton.value;
  setTimeout(() => {
    chargeSign.value = !chargeSign.value;
    //showHidebutton.value = !showHidebutton.value;
    signIn();
  }, 3000);
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Mono:ital@0;1&display=swap");
</style>
