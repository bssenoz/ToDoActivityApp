<template>
  <v-container fluid class="registration-container" :style="{ backgroundImage: backgroundImage }">
    <v-row>
      <v-col cols="7"></v-col>
      <v-col cols="5">
        <div class="form-container">
          <v-row  style="margin-top:10rem">
              <v-col>
                  <v-btn color="blue-darken-1" variant="text" href="/login">Giriş</v-btn>
                  <v-btn color="green-darken-1" variant="text" href="/register">Kayıt</v-btn>
              </v-col>
          </v-row>
 <v-row>
  <v-col>
      <v-text-field label="Mail" outlined variant="solo" v-model="email"></v-text-field>
      <v-text-field v-model="password" label="Şifre" type="password" required variant="solo"></v-text-field>
      <v-btn color="blue-darken-1" variant="text" @click="Login" style="width:100%">Giriş Yap</v-btn>
  </v-col>
 </v-row>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import Swal from 'sweetalert2';
import store from '@/store/index';

export default {
  name: "UserLogin",
  setup() {
    const router = useRouter();

    const backgroundImage = ref('');
      const email = ref('');
      const password = ref('');

    const setRandomBackground = () => {
      const backgrounds = [
      'url("https://www.tavsiyelist.com/wp-content/uploads/2018/07/Yemek.jpg")',
        'url("https://www.tavsiyelist.com/wp-content/uploads/2018/07/Yemek.jpg")',
        'url("https://www.tavsiyelist.com/wp-content/uploads/2018/07/Yemek.jpg")',
      ];
      const randomIndex = Math.floor(Math.random() * backgrounds.length);
      backgroundImage.value = backgrounds[randomIndex];
    };

    const Login = () => {
        axios.post('/api/Authentication/Login', {
            email: email.value,
            password: password.value,
        }) .then((res) => {
            if (res.status === 200) {
              const token = res.data.jwtTokenDTO.accessToken;
              localStorage.setItem("x-access-token",token);
              store.commit('setAccessToken', token);
              res.headers["authorization"] = `Bearer ${token}`;
              router.push('/profile');
            }
          })
          .catch((err) => {
          console.log(err)
          Swal.fire({
                title: 'Hatalı Giriş!',
                text:'Lütfen şifreni doğru girdiğinden emin ol.',
                icon: 'warning',
                confirmButtonText: 'Tamam',
              });
        })
     
    };

    onMounted(() => {
      setRandomBackground();
    });

    return {
      backgroundImage,
      email,
      password,
      Login
    };
  },
};
</script>

<style scoped>
.registration-container {
  background-size: cover;
  height: 100vh;
  position: relative;
}
.form-container{
   text-align: center;
   padding-left: 10rem;
   padding-right: 8rem;
}
.form-container::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 60%;
  background: rgba(0, 0, 0, 0.5);
}

</style>
