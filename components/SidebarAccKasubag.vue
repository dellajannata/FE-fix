<template>
  <SidebarPerRole>
    <template #dataDashboard>
    <NuxtLink class="nav-link" to="/dashboard_kasubag">
        <i class="mdi mdi-grid-large menu-icon"></i>
          <span class="menu-title">Dashboard</span>
    </NuxtLink>
    </template>
    <template #dataForm>
      <NuxtLink class="nav-link" to="/pengajuan_cuti_acc_kasubag_umum">
        <i class="menu-icon mdi mdi-file-document"></i>
        <span class="menu-title">Pengajuan Cuti Kasubag Umum</span>
      </NuxtLink>
    </template>
    <template #dataRekap>
      <NuxtLink class="nav-link" to="/rekap_cuti_acc_kasubag">
        <i class="menu-icon mdi mdi-file-document"></i>
        <span class="menu-title">Data Rekap Cuti</span>
      </NuxtLink>
    </template>
    <template #dataPengguna>
      <NuxtLink class="nav-link" data-bs-toggle="collapse" href="/pengguna_kasubag" aria-expanded="false" aria-controls="auth">
        <i class="menu-icon mdi mdi-account-circle-outline"></i>
        <span class="menu-title">Pengguna</span>
      </NuxtLink>
    </template>
  </SidebarPerRole>
</template>

<script>
import SidebarPerRole from './SidebarPerRole.vue';
</script>