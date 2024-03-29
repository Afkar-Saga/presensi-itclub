<template>
  <div>
    <h1>Member</h1>
    <div v-if="user">
      <NuxtLink to="/member/tambah">Tambah Member</NuxtLink>
      <NuxtLink to="/admin">Dashboard</NuxtLink>
    </div>
    <table border="1" width="100%">
      <thead>
        <tr>
          <th>No</th>
          <th>Nama</th>
          <th>Kelas</th>
          <th v-if="user">Token</th>
          <th v-if="user">Edit</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(member, index) in members" :key="member.id">
          <td>{{ index + 1 }}</td>
          <td>{{ member.nama }}</td>
          <td>{{ member.kelas }}</td>
          <td v-if="user">{{ member.token }}</td>
          <td v-if="user">
            <UButton label="✏️" @click="openEditMode(member.id)" />
          </td>
        </tr>
      </tbody>
    </table>
    <UModal v-model="editMode" prevent-close>
      <UCard>
        <template #header>
          Edit Member
        </template>

        <UInput color="green" variant="outline" v-model="member.nama" placeholder="Nama" />
        <UInput color="green" variant="outline" v-model="member.kelas" placeholder="Kelas" />

        <template #footer>
          <UButton label="Update" @click="editMember(member.id)" />
        </template>
      </UCard>
    </UModal>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const user = useSupabaseUser()

const members = ref([])
const member = ref({
  id: '',
  nama: '',
  kelas: ''
})

const fetchMembers = async () => {
  const { data, error } = await supabase.from('member').select('*')
  if (error) throw error
  if (data) members.value = data
}

const editMode = ref(false)

const openEditMode = async (memberId) => {
  editMode.value = true
  const { data, error } = await supabase.from('member').select('id, nama, kelas').eq('id', memberId).limit(1).single()
  if (error) throw error
  if (data) member.value = data
}

const editMember = async (memberId) => {
  try {
    const { data, error } = await supabase
    .from('member')
    .update({
      nama: member.value.nama,
      kelas: member.value.kelas
    })
    .eq('id', memberId)
    .select()
    if (error) throw error
    if (data) {
      console.log(data)
      fetchMembers()
    }
  } catch(error) {
    alert(error.errorDescription || error.message)
  } finally {
    editMode.value = false
  }
}

onMounted(() => {
  fetchMembers()
})
</script>