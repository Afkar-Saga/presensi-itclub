<template>
  <div>
    <form @submit.prevent.once="login">
      <input v-model="email" type="email" placeholder="Email">
      <input v-model="password" type="password" placeholder="Password">
      <input type="submit" value="Login">
    </form>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const user = useSupabaseUser()

const loading = ref(false)
const email = ref('')
const password = ref('')

const login = async () => {
  try {
    loading.value = true
    const { error } = await supabase.auth.signInWithPassword({
      email: email.value,
      password: password.value
    })
    email.value = ''
    password.value = ''
    if (error) throw error
  } catch (error) {
    alert(error.error_decsription || error.message)
  } finally {
    loading.value = false
  }
}

watchEffect(() => {
  if (user.value) {
    return navigateTo('/admin')
  }
})

console.log(user.value)
</script>

<style>

</style>