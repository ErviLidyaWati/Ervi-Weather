<template>
    <nav class="navbar">
      <div class="navbar-brand">
        <router-link :to="homeLink" class="navbar-item">{{ appName }}</router-link>
      </div>
      <div class="task-links">
        <div class="dropdown" @click="toggleDropdown">
          <div class="navbar-item">ERVI'S TASKS ▼</div> 
          <div class="dropdown-menu" :class="{ 'is-active': isDropdownActive }">
            <a href="https://ervi-project-cv.web.app/" class="navbar-item" target="_blank">TASK 1</a>
            <a href="https://ervi-223510619-projectpbk2.web.app/" class="navbar-item" target="_blank">TASK 2</a>
            <a href="https://ervi-223510619-todolist-t3pbk.web.app/" class="navbar-item" target="_blank">TASK 3</a>
            <a href="https://ervilidyawati-223510619-tugas4.web.app/" class="navbar-item" target="_blank">TASK 4</a>
            <a href="https://ervi-223510619-t5.web.app/" class="navbar-item" target="_blank">TASK 5</a>
            <a href="https://ervi-223510619-t6.web.app/" class="navbar-item" target="_blank">TASK 6</a>
            <a href="https://ervi-todolist-t7.web.app/" class="navbar-item" target="_blank">TASK 7</a>
          </div>
        </div>
      </div>
    </nav>
  </template>
  
  <script>
  export default {
    name: 'Navbar',
    props: {
      appName: {
        type: String,
        default: 'WEATHER' // Nilai default untuk judul aplikasi
      },
      homeLink: {
        type: String,
        default: '/' // Nilai default untuk link halaman utama
      }
    },
    data() {
      return {
        isDropdownActive: false
      };
    },
    methods: {
      toggleDropdown() {
        this.isDropdownActive = !this.isDropdownActive;
      }
    }
  };
  </script>
  
  <style scoped>
  .navbar {
    background-color: #023147; /* Warna latar belakang navbar */
    color: #82daf7; /* Warna teks */
    padding: 10px 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 4px 8px rgba(157, 253, 254, 0.2); /* Bayangan untuk efek visual */
  }
  
  .navbar-brand {
    display: flex;
    align-items: center;
  }
  
  .navbar-item {
    color: #bc0d0d;
    text-decoration: none;
    margin: 0 10px;
    padding: 10px;
    border-radius: 5px;
    transition: background-color 0.3s, color 0.3s;
    font-size: 18px;
    font-weight: bold; /* Menambahkan ketebalan teks */
    letter-spacing: 1px; /* Menambahkan jarak antar huruf */
  }
  
  .navbar-item:hover {
    background-color: #09def5; /* Warna latar belakang saat dihover */
  }
  
  .task-links {
    display: flex;
    align-items: center;
  }
  
  .dropdown {
    position: relative;
    cursor: pointer;
  }
  
  .dropdown-menu {
    display: none;
    position: absolute;
    top: calc(100% + 10px);
    left: 50%;
    transform: translateX(-50%);
    background-color: #031e3a; /* Warna latar belakang dropdown */
    border-radius: 10px;
    padding: 10px 0;
    box-shadow: 0 4px 20px rgba(137, 172, 248, 0.1);
    z-index: 1000;
    width: 200px;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
  }
  
  .dropdown-menu.is-active {
    display: block;
    opacity: 1;
    pointer-events: auto;
  }
  
  .dropdown-menu .navbar-item {
    display: block;
    width: 100%;
    padding: 10px 20px;
    font-size: 16px;
  }
  
  .dropdown-menu .navbar-item:hover {
    background-color: #87bdf5; /* Warna latar belakang saat dihover */
  }
  
  .navbar-item.active {
    background-color: #8fbeff;
  }
  </style>
  