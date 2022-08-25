<template>
  <div>
    <router-view />
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import { onMounted } from "vue";
import { storeToRefs } from "pinia";
import { useUserStore } from "./stores/user.js";

const router = useRouter();
const userStore = useUserStore();
const { user } = storeToRefs(userStore);

onMounted(async () => {
  const appReady = ref(null);
  try {
    //fetch to all users
    await userStore.fetchUser(); 
    if (!user.value) {
    //logout once not connected
      appReady.value = true;
      router.push({ path: "/auth/login" });
    } else {
    }
  } catch (e) {
    console.log(e);
  }
});
</script>

<style>
body {
  padding: 0;
  margin: 0;
  font-family: "Roboto", sans-serif;
}
</style>
