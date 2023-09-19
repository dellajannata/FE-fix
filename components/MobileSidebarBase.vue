<template>
  <div class="sidebar">
    <ul class="nav">
      <li class="nav-item">
        <slot name="dashboard"></slot>
      </li>
      <li class="nav-item nav-category ml-2">Data Dan Form</li>
      <slot name="data-dan-form"></slot>
      <li class="nav-item nav-category ml-2">Data Pengguna</li>
      <li class="nav-item">
        <slot name="user"></slot>
      </li>
      <li class="nav-item">
        <NuxtLink class="nav-link" @click="logout">
          <i class="menu-icon mdi mdi-logout"></i>
          <span class="menu-title">Logout</span>
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script>
import Swal from 'sweetalert2';

export default {
  methods: {
    async logout() {
      try {
        const result = await Swal.fire({
          title: 'Apakah Anda yakin akan keluar?',
          icon: 'question',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Keluar'
        });

        if (result.isConfirmed) {
          if (!this.rememberMe) {
            localStorage.removeItem('token');
            localStorage.removeItem('user');
            this.backLogin()
          }
        }
      } catch (error) {
        console.error(error);
        Swal.fire({
          icon: 'warning',
          title: 'Gagal Logout',
          text: 'Coba ulangi.',
        });
      };
    },
    backLogin() {
      this.$router.push('/login');
    },
  }
}
</script>
  
<style scoped>
.sidebar {
  width: 100%;
  min-height: 100%;
  background-color: white;
  display: block;
  padding: 12px 0;
}

:deep(.nav-link) {
  padding-left: 12px !important;
}

:deep(a.router-link-active) {
  background: #B1141D;
}

:deep(a.router-link-active *) {
  color: #fff !important;
}
</style>