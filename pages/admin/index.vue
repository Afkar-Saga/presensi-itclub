<template>
  <div>
    <h2>Welcome to Dashboard</h2>
    <NuxtLink to="/member">Cek Member</NuxtLink>
    <button @click="logout">Logout</button>
  </div>
</template>

<script setup>
definePageMeta({
  middleware: 'auth'
})
const supabase = useSupabaseClient()
const user = useSupabaseUser()

const logout = async () => {
  const { error } = await supabase.auth.signOut()
  if (error) throw error
}

watchEffect(() => {
  if (!user.value) {
    return navigateTo('/')
  }
})
</script>

<style>

</style>