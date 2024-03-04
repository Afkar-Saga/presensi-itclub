<template>
  <div>
    <table border="1" width="100%">
      <thead>
        <tr>
          <th>No</th>
          <th>Tanggal</th>
          <th>Nama</th>
          <th>Kelas</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(member, index) in kehadiran" :key="member.id">
          <td>{{ index + 1 }}</td>
          <td>{{ member.tanggal }}</td>
          <td>{{ member.member.nama }}</td>
          <td>{{ member.member.kelas }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const { data: kehadiran } = await useAsyncData('kehadiran', async() => {
  const { data } = await supabase.from('kehadiran').select(`
    tanggal,
    member ( * )
  `)
  return data
})
</script>