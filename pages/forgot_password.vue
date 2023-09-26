<template>
  <div class="limiter">
    <div class="container-login100">
      <div class="wrap-login100">
        <form class="login100-form validate-form" @submit.prevent="onLogin()">
          <div class="wrap-input100 validate-input m-b-26" data-validate="Username is required">
            <span class="label-input100">Enter Your E-mail to Change Password</span>
            <input class="input100" v-model="form.email" type="email" name="email" placeholder="Enter Email" />
            <span class="focus-input100"></span>
          </div>
          <div class="container-login100-form-btn">
            <button class="login100-form-btn" type="submit">Send</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import Swal from "sweetalert2";

definePageMeta({
  layout: "blank",
});

const { $sanctumAuth, $apiFetch } = useNuxtApp();
const router = useRouter();
const appConfig = useAppConfig();

const error = ref(""); // Changed to a single error message

const form = ref({
  email: "",
});

const REDIRECT_ROUTES = {
  ADMIN: "/dashboard",
  KABID: "/dashboard_kabid",
  KASUBAG: "/dashboard_kasubag",
  KADIS: "/dashboard_kadis",
  SEKRETARIS: "/dashboard_sekretaris",
  PEGAWAI: "/dashboard_pegawai",
};

const onLogin = async () => {
  router.push("/new_password");
};
</script>
