<template>
  <div class="container">
    <div class="left-column">
      <iframe width="560" height="315" src="https://www.youtube.com/embed/VIDEO_ID" frameborder="0" allowfullscreen></iframe>
    </div>
    <div class="right-column">
      <h1>Book a Table</h1>
      <form @submit.prevent="bookTable">
        <div class="form-row-1">
          <label for="name">Name:</label>
          <input type="text" id="name" v-model="bookingData.name" required>
          
          <label for="email">Email:</label>
          <input type="email" id="email" v-model="bookingData.email" required>
        </div>
        <div class="form-row-2">
          <label for="date">Date:</label>
          <input type="date" id="date" v-model="bookingData.date" required>
          
          <label for="numberOfPeople">Number of People:</label>
          <input type="number" id="numberOfPeople" v-model="bookingData.numberOfPeople" required>
        </div>
        <div class="form-row-3">
          <label for="specialRequests">Special Requests:</label>
          <textarea id="specialRequests" v-model="bookingData.specialRequests"></textarea>
        </div>

        <div class="form-row-4">
        <button type="submit">Book Now</button>
        </div>

        
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bookingData: {
        id: 1, 
        name: '',
        email: '',
        date: '',
        numberOfPeople: 1,
        specialRequests: ''
      },
      submitted: false
    };
  },
  methods: {
    bookTable() {
      this.bookingData.id++;
      
      fetch('http://localhost:5174/book', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.bookingData)
      })
      .then(response => {
        console.log(response);
        this.submitted = true; 
        this.resetForm(); 
      })
      .catch(error => {
        console.error('Error:', error);
      });
    },
    resetForm() {
      this.bookingData = {
        id: this.bookingData.id, 
        name: '',
        email: '',
        date: '',
        numberOfPeople: 1,
        specialRequests: ''
      };
      this.submitted = false; 
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
}

.left-column {
  flex: 1;
  padding: 20px;
}

.right-column {
  flex: 1;
  padding: 10px;
  background-color: transparent;
  
}
h1{
    margin-bottom: 10px;
}
.form-row-1 {
  display: flex;
  justify-content: space-evenly;
  margin-bottom: 20px;
}
.form-row-2 {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.form-row-3 {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.form-row-4 {
  display: flex;

}


.form-row-1 label {
  flex: 1;
  margin-right: 40px;
}
.form-row-2 label {
  flex: 1;
  margin-right: 5px;
}
.form-row-3 label {
  flex: 1;
  margin-right: 20px;
}

.form-row-1 input,
.form-row-1 textarea {
  flex: 2;
  background-color: whitesmoke;
  border-style: none;
  border-radius: 10px;


}

.form-row-2 input,
.form-row-2 textarea {
  flex: 2;
  background-color: whitesmoke;
  border-style: none;
  border-radius: 10px;
}

.form-row-3 input,
.form-row-3 textarea {
  flex: 10;
  background-color: whitesmoke;
  border-style: none;
  border-radius: 10px;
}

button {
    flex: 10;
    height: 30px;
  background-color: #a40518;
  color: black;
  text-decoration: none;
  border: 2px solid #a40518;
  border-radius: 5px;
  font-weight: bold;
  transition: background-color 0.3s, color 0.3s;
}

button:hover {
  background-color: transparent;
  color: black;
}
</style>
