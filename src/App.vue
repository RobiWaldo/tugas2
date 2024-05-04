<template>
  <div class="background">
    <img src="https://i.ibb.co/3Szyhft/image.png" />
  </div>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="border p-4">
          <h2 class="text-center mb-4"style="color: white;">Pendaftaran Akun</h2>
          <div class="mb-3" :class="{ 'mb-4': validEmail, 'mb-3': !validEmail }">
            <label for="username" class="form-label"style="color: white;">Username:</label>
            <input type="text" id="username" class="form-control" v-model="username" @blur="lowercaseUsername">
          </div>
          <div class="mb-3" :class="{ 'mb-4': validEmail, 'mb-3': !validEmail }">
            <label for="email" class="form-label"style="color: white;">Email:</label>
            <input type="email" id="email" class="form-control" v-model="email" @blur="validateEmail">
            <small v-if="!validEmail" class="text">Email tidak valid</small>
          </div>
          <div class="mb-3" :class="{ 'mb-4': validPassword, 'mb-3': !validPassword }">
            <label for="password" class="form-label"style="color: white;">Password:</label>
            <div class="input-group">
              <input :type="passwordVisible ? 'text' : 'password'" id="password" class="form-control" v-model="password" @input="checkPassword">
              <button class="btn btn-outline-secondary" type="button" @click="togglePasswordVisibility">
                <span v-if="passwordVisible" style="color: white;">Sembunyikan</span>
                <span v-else style="color: white;">Tampilkan</span>
              </button>
            </div>
            <ul :class="{ 'text-success': validPassword }" v-if="!validPassword" class="text " style="transition: color 0.3s">
              <li>mengandung angka</li>
              <li>mengandung huruf kapital</li>
            </ul>
          </div>
          <div class="mb-3" :class="{ 'mb-4': password === confirmPassword, 'mb-3': password !== confirmPassword }">
            <label for="confirmPassword" class="form-label" style="color: white;">Ulangi Password:</label>
            <div class="input-group">
              <input :type="confirmPasswordVisible ? 'text' : 'password'" id="confirmPassword" class="form-control" v-model="confirmPassword">
              <button class="btn btn-outline-secondary" type="button" @click="toggleConfirmPasswordVisibility">
                <span v-if="confirmPasswordVisible" style="color: white;">Sembunyikan</span>
                <span v-else style="color: white;">Tampilkan</span>
              </button>
            </div>
            <small v-if="password !== confirmPassword" class="text">Password tidak cocok</small>
          </div>
          <button class="btn btn-primary btn-block" @click="register">Daftar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Swal from 'sweetalert2';
import { ref } from 'vue';

const username = ref('');
const email = ref('');
const password = ref('');
const confirmPassword = ref('');
const validEmail = ref(true);
const validPassword = ref(true);
const buttonDisabled = ref(true);
const passwordVisible = ref(false);
const confirmPasswordVisible = ref(false);

const lowercaseUsername = () => {
  username.value = username.value.toLowerCase();
  validateButton();
};

const validateEmail = () => {
  validEmail.value = /\S+@\S+\.\S+/.test(email.value);
  validateButton();
};

const checkPassword = () => {
  const hasNumber = /\d/.test(password.value);
  const hasCapital = /[A-Z]/.test(password.value);
  validPassword.value = hasNumber && hasCapital;
  validateButton();
};

const validateButton = () => {
  buttonDisabled.value = !(validEmail.value && validPassword.value && password.value === confirmPassword.value);
};

const register = () => {
  if (validEmail.value && validPassword.value && password.value === confirmPassword.value) {
    Swal.fire({
      title: 'Akun berhasil dibuat!',
      icon: 'success',
      confirmButtonText: 'OK'
    });
  }
};

const togglePasswordVisibility = () => {
  passwordVisible.value = !passwordVisible.value;
};

const toggleConfirmPasswordVisibility = () => {
  confirmPasswordVisible.value = !confirmPasswordVisible.value;
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  position: fixed;
  left: 1%;
  top: 1%;
}
.container label, input{
  display: flex;
}
.container input{
  background-color: transparent;
  color: white;
  backdrop-filter: blur(5px);
}
.p-4{
  padding-left: 20%;
}

.border {
  border: 1px solid #ccc;
  border-radius: 8px;
  width: 200%;
}

.text {
  color: red;
  text-shadow: 0 0 50 white;
  font-size: 0.875rem;
  text-align: left;
  text-shadow: 10px 10px 10px;
  backdrop-filter: blur(5px);
}

small{
  text-shadow: 10px 10px 10px;
  backdrop-filter: blur(5px);
}

.mb-3 {
  margin-bottom: 1rem;
}

.mb-4 {
  margin-bottom: 1.5rem;
}

.text-success {
  color: green;
}

.background img{
  position: fixed;
  top: 0;
  left: 0;
  min-height: 90%;
  min-width: 1500px;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -2;
  object-fit: cover;
  -webkit-background-size:cover; -moz-background-size:cover; -o-background-size:cover; background-size: cover;
  filter: brightness(0.8);
}
</style>
