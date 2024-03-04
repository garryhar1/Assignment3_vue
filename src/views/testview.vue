<template>
  <div class="container">
    <div class="menu-container">
      <h1>Most Popular Menu</h1>
      <button @click="showModal = true" class="add-menu-btn">Add Menu</button>
    <div v-if="showModal" class="modal-background">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <h2>{{ modalTitle }}</h2>
        <form @submit.prevent="submitForm">
          <div class="form-group">
            <label for="menuName">Menu Name:</label>
            <input type="text" v-model="formData.name" required>
          </div>
          <div class="form-group">
            <label for="menuDescription">Menu Description:</label>
            <textarea v-model="formData.description" required></textarea>
          </div>
          <div class="form-group">
            <label for="menuPrice">Menu Price:</label>
            <input type="number" v-model="formData.price" required>
          </div>
          <div class="form-group">
            <label for="menuImage">Menu Image URL:</label>
            <input type="text" v-model="formData.image" required>
          </div>
          <div class="button-group">
            <button type="submit" class="submit-btn">{{ submitButtonText }}</button>
            <button type="button" @click="closeModal" class="cancel-btn">Cancel</button>
          </div>
        </form>
      </div>
    </div>
      <div class="menu-list">
        <div v-for="(page, index) in paginatedMenu" :key="index" v-show="currentPage === index">
          <div class="menu-page">
            <div class="menu-column">
              <div v-for="(item, i) in page.column1" :key="i" class="menu-item">
                <img :src="item.image" alt="Menu Item Image" class="menu-item-image">
                <div class="menu-item-details">
                  <div class="text-content">
                    <h3>{{ item.name }}</h3>
                    <p>{{ item.description }}</p>
                    <p class="price">Price: ${{ item.price }}</p>
                    <button @click="editMenu(item)" class="update-menu-btn">Update Menu</button>
                  </div>
                </div>
              </div>
            </div>
            <div class="menu-column">
              <div v-for="(item, i) in page.column2" :key="i" class="menu-item">
                <img :src="item.image" alt="Menu Item Image" class="menu-item-image">
                <div class="menu-item-details">
                  <div class="text-content">
                    <h3>{{ item.name }}</h3>
                    <p>{{ item.description }}</p>
                    <p class="price">Price: ${{ item.price }}</p>
                    <button @click="editMenu(item)" class="update-menu-btn">Update Menu</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="pagination">
          <button @click="previousPage" :disabled="currentPage === 0">&lt;</button>
          <button v-for="pageNumber in totalPages" :key="pageNumber" @click="goToPage(pageNumber - 1)" :class="{ 'active': currentPage === pageNumber - 1 }">{{ pageNumber }}</button>
          <button @click="nextPage" :disabled="currentPage === totalPages - 1">&gt;</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import menuData from '../../data.json';

export default {
  name: 'MenuList',
  data() {
    return {
      menus: menuData.new,
      itemsPerPage: 6,
      currentPage: 0,
      showModal: false,
      modalTitle: 'Add New Menu',
      submitButtonText: 'Add Menu',
      formData: {
        name: '',
        description: '',
        price: '',
        image: ''
      },
      editingMenu: null
    };
  },
  computed: {
    paginatedMenu() {
      const start = this.currentPage * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      const slicedItems = this.menus.slice(start, end);
      const paginated = [];
      for (let i = 0; i < slicedItems.length; i += 6) {
        const column1 = slicedItems.slice(i, i + 3);
        const column2 = slicedItems.slice(i + 3, i + 6);
        paginated.push({
          column1,
          column2
        });
      }
      return paginated;
    },
    totalPages() {
      return Math.ceil(this.menus.length / this.itemsPerPage);
    }
  },
  methods: {
    goToPage(pageNumber) {
      this.currentPage = pageNumber;
    },
    nextPage() {
      if (this.currentPage < this.totalPages - 1) {
        this.currentPage++;
      }
    },
    previousPage() {
      if (this.currentPage > 0) {
        this.currentPage--;
      }
    },
    showModalDialog(title, buttonText, data = null) {
      this.modalTitle = title;
      this.submitButtonText = buttonText;
      this.formData = data ? { ...data } : { name: '', description: '', price: '', image: '' };
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.editingMenu = null;
    },
    submitForm() {
      if (this.editingMenu) {
        this.updateMenu();
      } else {
        this.addMenu();
      }
    },
    addMenu() {
      fetch('http://localhost:5174/new', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.formData)
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Error adding menu');
        }
        return response.json();
      })
      .then(data => {
        alert('Menu added successfully!', data);
        this.resetForm();
        this.closeModal();
      })
      .catch(error => {
        console.error(error);
        alert('Error adding menu');
      });
    },
    updateMenu() {
      fetch(`http://localhost:5174/new/${this.editingMenu.id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.formData)
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Error updating menu');
        }
        return response.json();
      })
      .then(data => {
        alert('Menu updated successfully!', data);
        this.resetForm();
        this.closeModal();
      })
      .catch(error => {
        console.error(error);
        alert('Error updating menu');
      });
    },
    resetForm() {
      this.formData = {
        name: '',
        description: '',
        price: '',
        image: ''
      };
    },
    editMenu(menu) {
      this.editingMenu = menu;
      this.showModalDialog('Update Menu', 'Update Menu', menu);
    }
  }
};
</script>

<style scoped>
.container {
  padding-top: 50px;
  width: 1200px;
  display: fixed;
  justify-content: center;
  align-items: center;
}

.menu-container {
  align-items: center;
  justify-content: center;
}

.menu-list {
  padding: 20px;
}

.menu-list h1 {
  margin-bottom: 20px;
}

.menu-page {
  display: flex;
  justify-content: space-between;
}

.menu-column {
  flex: 1;
}

.menu-item {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.menu-item-image {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.menu-item-details {
  flex: 1;
}

.text-content {
  flex: 1;
}

.price {
  font-weight: bold;
}

.pagination {
  margin-top: 20px;
  display: flex;
  justify-content: center;
}

.pagination button {
  background-color: transparent;
  border: none;
  padding: 5px 10px;
  margin: 0 5px;
  cursor: pointer;
}

.pagination button.active {
  background-color: black;
  color: white;
}

.pagination button:disabled {
  cursor: not-allowed;
}

.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); 
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.3); 
  max-width: 400px;
  width: 90%;
  position: relative;
  animation: fadeIn 0.3s ease-out;
}

.close {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
  font-size: 1.2em;
}

.form-group {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
}

.button-group {
  display: flex;
  justify-content: flex-end;
}

.submit-btn {
  background-color: black;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.cancel-btn {
  background-color: #f44336;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 10px;
}

.add-menu-btn {
  background-color: black; 
  color: white; 
  padding: 10px 15px; 
  border: none; 
  border-radius: 5px; 
  cursor: pointer; 
  transition: background-color 0.3s; 
}

.add-menu-btn:hover {
  background-color: transparent; 
}

.update-menu-btn {
  background-color: black;
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 5px;
}
</style>
