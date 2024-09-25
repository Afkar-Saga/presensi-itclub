<template>
  <div class="container mx-auto p-4">
    <form @submit.prevent="isiKehadiran" class="max-w-lg mx-auto bg-white p-6 border border-gray-200 rounded-lg shadow-lg">
      <h2 class="text-2xl text-black text-center font-bold mb-6">FORM PRESENSI</h2>

      <div class="mb-4">
        <label class="block text-gray-700 font-semibold mb-2">Nama</label>
        <select v-model="member" @change="getTokenByMember" class="w-full border border-gray-300 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-blue-500">
          <option disabled value="">Select member</option>
          <option :value="member.id" v-for="member in members">{{ member.nama }}</option>
        </select>
      </div>

      <div class="mb-4">
        <label class="block text-gray-700 font-semibold mb-2">Token</label>
        <input type="text" maxlength="6" v-model="token" :disabled="!member" class="w-full border border-gray-300 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-blue-500" />
      </div>

      <button type="submit" class="w-full bg-blue-500 text-white py-2 px-4 rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500">Absen</button>
    </form>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
let kelas = ref();
let member = ref();
let token = ref();
let tokenMember = ref();
let memberKelas = ref([]);

async function getMemberByKelas() {
  const { data, error } = await supabase.from("kelas").select().eq("kelas", kelas.value);
  memberKelas.value = data;
}

async function getTokenByMember() {
  const { data, error } = await supabase.from("member").select("token").eq("id", member.value);
  tokenMember.value = data[0].token;
}

const { data: members } = await useAsyncData("members", async () => {
  const { data } = await supabase.from("member").select();
  return data;
});

async function isiKehadiran() {
  // if (token.value == tokenMember.value) {
  //   const { error } = await supabase.from("kehadiran").insert([{ member: member.value }]);
  //   if (!error) {
  //     const { error } = await supabase
  //       .from("member")
  //       .update({
  //         token: Math.random().toString(36).substring(2).slice(0, 6),
  //       })
  //       .eq("id", member.value);
  //     if (error) console.log(error);
  //   }
  //   navigateTo("/kehadiran");
  // } else {
  //   alert("Token salah!");
  // }
  try {
    if (token.value == tokenMember.value) {
      const { error } = await supabase.from("kehadiran").insert({
        member: member.value
      })
      if (error) throw error
      else {
        const { error } = await supabase.from("member").update({
          token: Math.random().toString(36).substring(2).slice(0, 6),
        }).eq("id", member.value)
        if (error) throw error
        else navigateTo("/kehadiran")
      }
    } else {
      alert("Token salah!")
    }
  } catch (error) {
    console.error(error)
  }
}
</script>
