<template>
  <form @submit.prevent="isiKehadiran">
    <select v-model="kelas" @change="getMemberByKelas">
      <option disabled value="">Kelas?</option>
      <option v-for="member in members">{{ member.kelas }}</option>
    </select>
    <select v-model="member" :disabled="!kelas" @change="getTokenByMember">
      <option disabled value="">Select member</option>
      <option :value="member.id" v-for="member in memberKelas">{{ member.nama }}</option>
    </select>
    <input type="text" maxlength="6" v-model="token" :disabled="!member">
    <input type="submit" :disabled="!kelas || !member" value="Absen">
  </form>
</template>

<script setup lang="ts">
const supabase = useSupabaseClient()
let kelas = ref()
let member = ref()
let token = ref()
let tokenMember = ref()
let memberKelas = ref([])

async function getMemberByKelas() {
  const { data, error } = await supabase.from('member').select().eq('kelas', kelas.value)
  memberKelas.value = data
}

async function getTokenByMember() {
  const { data, error } = await supabase.from('member').select('token').eq('id', member.value)
  tokenMember.value = data[0].token
}

const { data: members } = await useAsyncData('members', async() => {
  const { data } = await supabase.from('member').select()
  return data
})

async function isiKehadiran() {
  if (token.value == tokenMember.value) {
    const { error } = await supabase.from('kehadiran').insert([
      { member: member.value }
    ])
    if (!error) {
      const { error } = await supabase.from('member').update({
        token: Math.random().toString(36).substring(2).slice(0, 6)
      }).eq('id', member.value)
      if (error) console.log(error)
    }
    navigateTo('/kehadiran')
  } else {
    alert('Token salah!')
  }
}
</script>