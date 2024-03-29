<template>
  <div>
    <h1>Tambah Member Baru</h1>
    <form @submit.prevent="simpan()">
      <input v-model="nama" placeholder="Nama"> <br>
      <input v-model="kelas" placeholder="Kelas"> <br>
      <button type="submit">Kirim</button>
    </form>
    <NuxtLink to="/member">Members</NuxtLink>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const nama = ref()
const kelas = ref()

async function simpan() {
  await supabase
  .from("member")
  .insert({
    nama: nama.value,
    kelas: kelas.value,
    token: Math.random().toString(36).substring(2).slice(0, 6)
  })
  navigateTo("/member")
}
</script>