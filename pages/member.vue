<template>
  <div>
    <h1>Member</h1>
    <nuxt-link to="/tambahMember">Tambah Member</nuxt-link>
    <table border="1" width="100%">
      <thead>
        <tr>
          <th>No</th>
          <th>Nama</th>
          <th>Kelas</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(member, index) in members" :key="member.id">
          <td>{{ index + 1 }}</td>
          <td>{{ member.nama }}</td>
          <td>{{ member.kelas }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const { data: members } = await useAsyncData('members', async() => {
  const { data } = await supabase.from('member').select()
  return data
})
</script>