<template>
  <!-- <div>
    <form @submit.prevent.once="login">
      <input v-model="email" type="email" placeholder="Email">
      <input v-model="password" type="password" placeholder="Password">
      <input type="submit" value="Login">
    </form>
  </div> -->

  <div class="container mx-auto p-4 mt-24">
    <form @submit.prevent.once="login" class="max-w-lg mx-auto bg-white p-6 border border-gray-200 rounded-lg shadow-lg">
      <h2 class="text-2xl text-center text-black font-bold mb-6">LOGIN</h2>

      <div class="mb-4">
        <input v-model="email" type="email" placeholder="Email" class="w-full border border-gray-700 bg-white rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-blue-500" />
      </div>

      <div class="mb-4">
        <input v-model="password" type="password" placeholder="Password" class="w-full border border-gray-700 bg-white rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-blue-500" />
      </div>

      <button type="submit" class="w-full bg-blue-500 text-white py-2 px-4 rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500">Login</button>
    </form>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();

const loading = ref(false);
const email = ref("");
const password = ref("");

const login = async () => {
  try {
    loading.value = true;
    const { error } = await supabase.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    });
    email.value = "";
    password.value = "";
    if (error) throw error;
  } catch (error) {
    alert(error.error_decsription || error.message);
  } finally {
    loading.value = false;
  }
};

watchEffect(() => {
  if (user.value) {
    return navigateTo("/admin");
  }
});

console.log(user.value);
</script>

<style></style>
