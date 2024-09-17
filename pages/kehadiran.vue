<template>
  <!-- <div>
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
          <td>{{ member.member.kelas.nama }}</td>
        </tr>
      </tbody>
    </table>
  </div> -->

  <div class="container mx-auto p-4">
    <table class="min-w-full bg-white border border-gray-200">
      <thead>
        <tr class="bg-gray-100 border-b">
          <th class="py-2 px-4 text-left text-gray-600">No</th>
          <th class="py-2 px-4 text-left text-gray-600">Tanggal</th>
          <th class="py-2 px-4 text-left text-gray-600">Nama</th>
          <th class="py-2 px-4 text-left text-gray-600">Kelas</th>
        </tr>
      </thead>
      <tbody>
        <tr class="border-b bg-gray-50 hover:bg-gray-100" v-for="(member, index) in kehadiran" :key="member.id">
          <td class="py-2 px-4 text-gray-700">{{ index + 1 }}</td>
          <td class="py-2 px-4 text-gray-700">{{ member.tanggal }}</td>
          <td class="py-2 px-4 text-gray-700">{{ member.member.nama }}</td>
          <td class="py-2 px-4 text-gray-700">{{ member.member.kelas.nama }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();

const { data: kehadiran } = await useAsyncData("kehadiran", async () => {
  const { data } = await supabase.from("kehadiran").select(`
    tanggal,
    member ( 
      *,
      kelas (
        nama
      )
    )
  `);
  return data;
});
</script>
