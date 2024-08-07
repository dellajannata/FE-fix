<template>
  <div class="card">
    <div class="card-body">
      <h4 class="card-title">Data Pegajuan Cuti Pegawai</h4>
      <div class="card-pegawai">
        <div class="search__container">
          <input class="search__input" type="text" placeholder="Nama/unit kerja" v-model="searchQuery" @input="search">
          <i class="fa fa-search search__icon"></i>
        </div>
        <NuxtLink class="btn btn-primary" to="/create_pengajuan_cuti_pegawai"><i class="fa fa-plus"></i></NuxtLink>
      </div>
      <div class="table-responsive">
        <table class="table table-hover">
          <thead>
            <tr>
              <th>No.</th>
              <th>Nama</th>
              <th>Unit Kerja</th>
              <th>Tanggal Awal</th>
              <th>Tanggal Akhir</th>
              <th>Alasan</th>
              <th>Waktu Pengajuan</th>
              <th>Status</th>
              <th>Keterangan</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(cuti, index) in filteredCuti" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ getNamaPegawai() }}</td>
              <td><template v-if="getUnitKerja().split(' ').length > 3">
                  {{ getUnitKerja().split(' ').slice(0, 3).join(' ') }}<br><br>
                  {{ getUnitKerja().split(' ').slice(3).join(' ') }}
                </template>
                <template v-else>
                  {{ getUnitKerja() }}
                </template>
              </td>
              <td>{{ cuti.tgl_awal }}</td>
              <td>{{ cuti.tgl_akhir }}</td>
              <td>{{ cuti.alasan }}</td>
              <td>{{ waktu_pengajuan(cuti.created_at) }}</td>
              <td>{{ cuti.status }}</td>
              <td>
                <template v-if="cuti.status == 'Selesai'">
                  <button @click="$event => cetakPDF($event, cuti.id)" class="btn btn-success btn-sm">Cetak PDF</button>
                </template>
                <template v-else-if="cuti.status == 'Belum' | cuti.status.includes('ACC')">
                  Proses ACC
                </template>
                <template v-else>
                  Ditolak
                </template>
              </td>
              <td class="btn-action">
                <template
                  v-if="cuti.status != 'Selesai' && cuti.status != 'Ditolak Kabid' && cuti.status != 'Ditolak Kasubag Umum' && cuti.status != 'Ditolak Sekretaris' && cuti.status != 'Ditolak Kadis'">
                  <NuxtLink :to="`../editCuti_${cuti.id}`" class="btn btn-warning btn-sm"><i class="fa fa-pencil"></i>
                  </NuxtLink>
                </template>
                <button @click="deleteCuti(cuti.id)" class="btn btn-danger btn-sm"><i class="fa fa-trash"></i></button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Swal from 'sweetalert2';

export default {
  data() {
    return {
      data_cuti: [],
      data_pegawai: [],
      searchQuery: ""
    }
  },
  computed: {
    filteredCuti() {
      const idUser = this.getIdUserYangLogin(); // Mengambil nama pengguna dari localStorage 1

      // Filter data cuti berdasarkan nama pengguna yang login
      return this.data_cuti.filter(cuti => cuti.user_id === idUser);
    },
  },
  mounted() {
    this.getDataPengajuanCuti();
    this.getDataPegawai();
  },
  methods: {
    search() {
      if (this.searchQuery !== "") {
        const accessToken = localStorage.getItem('token');
        axios.get(`http://127.0.0.1:8000/api/pengajuan_cuti/search/${this.searchQuery}`, {
          headers: {
            'Authorization': `Bearer ${accessToken}`
          }
        })
          .then(res => {
            console.log(res.data.data);
            this.data_cuti = res.data.data;
          })
          .catch(error => {
            console.error('Error fetching data:', error);
          });
      } else {
        this.getDataPengajuanCuti();
      }
    },
    getIdUserYangLogin() {
      const userData = JSON.parse(localStorage.getItem('user'));
      return userData ? userData.id : ''; // Mengambil nama pengguna dari objek pengguna 
    },
    getDataPengajuanCuti() {
      const accessToken = localStorage.getItem('token');
      axios.get('http://127.0.0.1:8000/api/pengajuan_cuti_all', {
        headers: {
          'Authorization': `Bearer ${accessToken}`
        }
      })
        .then(res => {
          console.log(res.data.data);
          this.data_cuti = res.data.data;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    },
    getDataPegawai() {
      const accessToken = localStorage.getItem('token');
      axios.get('http://127.0.0.1:8000/api/pegawai_all', {
        headers: {
          'Authorization': `Bearer ${accessToken}`
        }
      }).then(res => {
        console.log(res.data.data);
        this.data_pegawai = res.data.data;
      })
        .catch(error => {
          console.error('Error fetching pegawai data:', error);
        });
    },
    getIdPegawai(pegawaiId) {
      const pegawai = this.data_pegawai.find(pegawai => pegawai.id === pegawaiId);
      return pegawai ? pegawai.id : 'Id Pegawai Tidak Tersedia';
    },
    getPegawaiId() {
      const userData = JSON.parse(localStorage.getItem('user'));
      return userData ? userData.pegawai_id : ''; // Mengambil id user dari objek pengguna 
    },
    getNamaPegawai() {
      const user = this.getPegawaiId();
      const pegawai = this.data_pegawai.find(pegawai => pegawai.id === user);
      return pegawai ? pegawai.nama : 'Nama Pegawai Tidak Tersedia';
    },
    getUnitKerja() {
      const user = this.getPegawaiId();
      const pegawai = this.data_pegawai.find(pegawai => pegawai.id === user);
      return pegawai ? pegawai.unit_kerja : 'Unit Kerja Pegawai Tidak Tersedia';
    },
    waktu_pengajuan(timestamp) {
      const jakartaTimeZone = 'Asia/Jakarta';
      const created_at = new Date(timestamp);
      const jakartaTime = new Date(created_at.toLocaleString("en-US", { timeZone: jakartaTimeZone }));

      // Opsi untuk menghilangkan zona waktu (GMT +7)
      const options = {
        year: "numeric",
        month: "2-digit",
        day: "2-digit",
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
        hour12: false // Menggunakan format 24 jam
      };
      const formattedTime = jakartaTime.toLocaleString("en-US", options).replace(/,/, '');
      return formattedTime.replace(/\//g, '-');
    },
    async deleteCuti(cutiId) {
      try {
        const result = await Swal.fire({
          title: 'Apakah Anda yakin akan menghapus data?',
          icon: 'question',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Ya, hapus data ini!'
        });

        if (result.isConfirmed) {
          const accessToken = localStorage.getItem('token');
          await axios.delete(`http://127.0.0.1:8000/api/pengajuan_cuti/${cutiId}`, {
            headers: {
              'Authorization': `Bearer ${accessToken}`
            }
          });
          Swal.fire(
            'Berhasil!',
            'Data Anda berhasil dihapus.',
            'success'
          );
          this.getDataPengajuanCuti();
        }
      } catch (error) {
        console.error(error);
      }
    },
    editCuti(cutiId) {
      axios.put(`http://127.0.0.1:8000/api/pengajuan_cuti/${cutiId}`).then(res => {
        this.getDataPengajuanCuti();
      });
    },

    cetakPDF(event, cutiId) {
      const accessToken = localStorage.getItem('token');
      axios.get(`http://127.0.0.1:8000/api/cetak-pdf/${cutiId}`, {
        headers: {
          'Authorization': `Bearer ${accessToken}`
        },
        responseType: 'blob'
      }).then(response => {
        const blob = new Blob([response.data], { type: 'application/pdf' });
        const url = window.URL.createObjectURL(blob);

        const link = document.createElement('a');
        link.href = url;
        link.target = '_blank';
        link.download = 'surat-permohonan-cuti.pdf';
        link.click();

        window.URL.revokeObjectURL(url);
      }).catch(error => {
        console.error('Error fetching PDF:', error);
      });

    }
  }
}
</script>