<template>
  <div id="app">
    <Navbar />
    <div class="content">
      <router-view />
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'

export default {
  name: 'App',
  components: {
    Navbar
  }
}
</script>

<style scoped>
/* CSS untuk mengatur tampilan halaman */
#app {
  background-color: #63bcf8;
}

/* CSS untuk judul aplikasi */


/* CSS untuk konten halaman */
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 20px; /* Ubah margin top */
}

/* CSS untuk tombol */
button {
  padding: 12px 24px; /* Ubah padding agar tombol lebih besar */
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Efek shadow */
}

button:hover {
  background-color: #0056b3;
}

/* CSS untuk input */
input {
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 15px; /* Sesuaikan margin */
  width: calc(100% - 24px); /* Sesuaikan dengan padding agar lebar tetap 100% */
  box-sizing: border-box;
  font-size: 1em; /* Ubah ukuran font */
}

/* CSS untuk informasi cuaca */
.weather-info {
  margin-top: 30px;
  width: 80%; /* Sesuaikan lebar agar tidak terlalu lebar */
  max-width: 600px; /* Batasi lebar maksimum */
}

/* CSS untuk informasi cuaca */
.weather-info p {
  margin: 8px 0; /* Ubah margin untuk ruang yang lebih baik */
  font-size: 0.9em; /* Sesuaikan ukuran font */
}

</style>
