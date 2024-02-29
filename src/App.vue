<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
          <router-link to="/" class="nav-link"><img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />
</router-link>
    <div class="nav-links">
      <router-link to="/" class="nav-link">Home</router-link>
      <router-link to="/menu" class="nav-link">Menu</router-link>
      <router-link to="/book" class="button-link nav-link">Book</router-link>      
    </div>
  </header>
  <router-view />
</template>

<style scoped>

header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px;
  background-color: transparent;
  color: white;
  
}

.logo {
  width: 50px;
  height: 20px;
}

.nav-links {
  display: flex;
}

.nav-link {
  margin-left: 20px;
  color: black;
  text-decoration: none;
}

.nav-link:hover {
  text-decoration: underline;
  background-color: transparent;
}

.button-link {
  padding: 2px 10px;
  background-color: #a40518; /* Green */
  color: white;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  border-radius: 5px;
}

.button-link:hover {
  background-color: black; 
}

main {
  margin-top: 80px;
}
</style>