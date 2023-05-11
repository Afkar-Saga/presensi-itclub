<template>
  <div>
    <h1>Member</h1>
    <NuxtLink to="/tambahMember">Tambah Member</NuxtLink>
    <table border="1" width="100%">
      <thead>
        <tr>
          <th>#</th>
          <th>Nama</th>
          <th>Kelas</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="member in datas" :key="member.id">
          <td>{{ member.id }}</td>
          <td>{{ member.nama }}</td>
          <td>{{ member.kelas }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const datas = ref([])

async function getData() {
  const { data, error } = await supabase
  .from("member")
  .select()
  datas.value = data
}

onMounted(() => {
  getData()
})
</script>