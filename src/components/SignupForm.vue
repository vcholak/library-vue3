<template>
  <form @submit.prevent="handleSubmit">
    <input
      v-model="displayName"
      type="text"
      required
      placeholder="display name"
    />
    <input v-model="email" type="email" required placeholder="email" />
    <input v-model="password" type="password" required placeholder="password" />
    <button>Sign Up</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { auth } from "../firebase/config";

const displayName = ref("");
const email = ref("");
const password = ref("");
const error = ref(null);

const handleSubmit = async () => {
  error.value = null;
  try {
    const resp = await auth.createUserWithEmailAndPassword(
      email.value,
      password.value
    );
    if (!resp) {
      throw new Error("Could not complete the signup");
    }
    await resp.user.updateProfile({ displayName: displayName.value });
    error.value = null;
    console.log(resp.user);
  } catch (err) {
    console.log(err.message);
    error.value = err.message;
  }
};
</script>

<style lang="scss" scoped></style>
