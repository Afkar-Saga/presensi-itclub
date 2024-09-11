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
          <th v-if="user">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(member, index) in members" :key="member.id">
          <td>{{ index + 1 }}</td>
          <td>{{ member.nama }}</td>
          <td>{{ member.kelas?.nama }}</td>
          <td v-if="user">{{ member.token }}</td>
          <td v-if="user">
            <UButton label="✏️" @click="openEditMode(member.id)" />
          </td>
          <td v-if="user">
            <UButton label="🗑️" @click="openDeleteMode(member.id)" />
          </td>
        </tr>
      </tbody>
    </table>
    <UModal v-model="editMode">
      <UCard>
        <template #header>
          Edit Member
        </template>

        <UInput color="green" variant="outline" v-model="member.nama" placeholder="Nama" />
        <UInput color="green" variant="outline" v-model="member.kelas" placeholder="Kelas" />

        <template #footer>
          <UButton label="Update" @click="editMember(member.id)" />
          <UButton label="Cancel" @click="editMode = false" />
        </template>
      </UCard>
    </UModal>
    <UModal v-model="deleteMode">
      <UCard>
        <template #header>
          Apakah Anda yakin ingin menghapus member ini?
        </template>

        <div>
          {{ member.nama }} - {{ member.kelas }}
        </div>

        <template #footer>
          <UButton label="Delete" @click="deleteMember(member.id)" />
          <UButton label="Cancel" @click="deleteMode = false" />
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
  const { data, error } = await supabase
    .from('member')
    .select(`
      id,
      nama,
      token,
      kelas (
        nama
      )
    `)
  if (error) throw error
  if (data) members.value = data
  console.log(data)
}

const editMode = ref(false)

const openEditMode = async (memberId) => {
  editMode.value = true
  const { data, error } = await supabase.from('member').select('id, nama, kelas').eq('id', memberId).limit(1).single()
  if (error) throw error
  if (data) member.value = data
}

const deleteMode = ref(false)

const openDeleteMode = async (memberId) => {
  deleteMode.value = true
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
  } catch (error) {
    alert(error.errorDescription || error.message)
  } finally {
    editMode.value = false
  }
}

const deleteMember = async (memberId) => {
  try {
    const { error } = await supabase.from('member').delete().eq('id', memberId)
    if (error) throw error
    else fetchMembers()
  } catch (error) {
    alert(error.errorDescription || error.message)
  } finally {
    deleteMode.value = false
  }
}

onMounted(() => {
  fetchMembers()
})
</script>