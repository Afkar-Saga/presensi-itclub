<template>
  <div class="flex items-center justify-center h-[80vh] space-x-4 text-center">
    <NuxtLink to="/member" class="w-1/4">
      <div class="bg-blue-500 text-white shadow-lg p-4 rounded-lg min-h-40">
        <NCard class="text-white">
          <NCardHeader>
            <h3 class="text-lg font-bold mt-10">MEMBER</h3>
          </NCardHeader>
        </NCard>
      </div>
    </NuxtLink>

    <NuxtLink to="/absen" class="w-1/4">
      <div class="bg-green-500 text-white shadow-lg p-4 rounded-lg min-h-40">
        <NCard class="text-white">
          <NCardHeader>
            <h3 class="text-lg font-bold mt-10">ABSEN</h3>
          </NCardHeader>
        </NCard>
      </div>
    </NuxtLink>

    <NuxtLink to="/kehadiran" class="w-1/4">
      <div class="bg-blue-500 text-white shadow-lg p-4 rounded-lg min-h-40">
        <NCard class="text-white">
          <NCardHeader>
            <h3 class="text-lg font-bold mt-10">CEK KEHADIRAN</h3>
          </NCardHeader>
        </NCard>
      </div>
    </NuxtLink>
  </div>
</template>

<style scoped></style>
