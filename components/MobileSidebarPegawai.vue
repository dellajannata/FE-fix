<template>
  <MobileSidebarBase>
    <template #dashboard>
      <NuxtLink class="nav-link" to="/dashboard_pegawai">
        <i class="mdi mdi-grid-large menu-icon"></i>
        <span class="menu-title">Dashboard</span>
      </NuxtLink>
    </template>
    <template #data-dan-form>
      <li class="nav-item">
        <NuxtLink class="nav-link" to="/data_pengajuan_cuti_pegawai">
          <i class="menu-icon mdi mdi-card-text-outline"></i>
          <span class="menu-title">Pengajuan Cuti</span>
          <!-- <i class="menu-arrow"></i> -->
        </NuxtLink>
      </li>
      <li class="nav-item">
        <NuxtLink class="nav-link" to="/data_rekap_cuti_pegawai">
          <i class="menu-icon mdi mdi-file-document"></i>
          <span class="menu-title">Rekap Cuti</span>
        </NuxtLink>
      </li>
    </template>
    <template #user>
      <NuxtLink class="nav-link" to="/pengguna_pegawai">
        <i class="menu-icon mdi mdi-account-circle-outline"></i>
        <span class="menu-title">Pengguna</span>
      </NuxtLink>
    </template>
  </MobileSidebarBase>
</template>

<script>
import MobileSidebarBase from './MobileSidebarBase.vue'
</script>
