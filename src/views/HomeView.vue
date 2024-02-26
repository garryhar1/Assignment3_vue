<template>
  <div class="container">
    <div class="content">
      <div class="text">
        <h1>Welcome to the V restaurant</h1>
        <h3>For our booking table can click the button below</h3>
        <router-link to="/book" class="button">Book Now</router-link>
      </div>
      <div class="image">
        <img src="@/assets/logo.svg" alt="Logo" />
      </div>
    </div>
  </div>
</template>


<style scoped>
.container {
  padding-top: 50px;
  width: 1200px;
  justify-content: center;
  align-items: center;
}

.content {
  display: flex;
  justify-content: space-around;
  align-items: center;
  max-width: 1300px;
  width: 80%;
}

.text {
  flex: 1;
  padding: 20px;
  color: black;
}

.image {
  
  flex: 1;
}

.image img {
  max-width: 80%;
  height: auto;
}

.button {
  display: inline-block;
  padding: 5px 15px;
  background-color: #a40518;
  color: black;
  text-decoration: none;
  border: 2px solid #a40518;
  border-radius: 5px;
  font-weight: bold;
  transition: background-color 0.3s, color 0.3s;
}

.button:hover {
  background-color: transparent;
  color: black;
}
</style>
