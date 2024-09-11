<template>
  <div>
    <h1>Tambah Member Baru</h1>
    <form @submit.prevent="simpan()">
      <UInput v-model="nama" placeholder="Nama" /> <br>
      Kelas: <select v-model="kelas">
        <option value="" disabled>Kelas</option>
        <option v-for="clas in classes" :value="clas.id">{{ clas.nama }}</option>
      </select> <br>
      <UButton type="submit">Kirim</UButton>
    </form>
    <NuxtLink to="/member">Members</NuxtLink>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const nama = ref()
const kelas = ref(0)
const classes = ref([])

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

async function fetchKelas() {
  const { data, error } = await supabase
  .from('kelas')
  .select('*')
  if (error) throw error
  if (data) {
    classes.value = data
  }
}

onMounted(() => {
  fetchKelas()
})
</script>