<template>
  <div class="container">
    <div class="menu-container">
      <h1>Most Popular Menu</h1>
      <div class="menu-choice">
        <button @click="selectedMenu = 'breakfast'" :class="{ 'active': selectedMenu === 'breakfast' }">Breakfast</button>
        <button @click="selectedMenu = 'lunch'" :class="{ 'active': selectedMenu === 'lunch' }">Lunch</button>
        <button @click="selectedMenu = 'dinner'" :class="{ 'active': selectedMenu === 'dinner' }">Dinner</button>
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
                    <p class="price">Price: {{ item.price }}</p>
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
                    <p class="price">Price: {{ item.price }}</p>
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
export default {
  name: 'MenuList',
  data() {
    return {
      selectedMenu: 'breakfast', // Default selected menu
      menus: {
        breakfast: [
          { name: 'Pancakes', description: 'Fluffy pancakes served with maple syrup', price: '$8', image: 'https://picsum.photos/200/300' },
          { name: 'Eggs Benedict', description: 'Poached eggs with hollandaise sauce on English muffin', price: '$10', image: 'https://picsum.photos/200/300' },
          { name: 'Avocado Toast', description: 'Sourdough toast topped with smashed avocado', price: '$9', image: 'https://picsum.photos/200/300' },
          { name: 'Waffles', description: 'Golden brown waffles served with fresh berries', price: '$8', image: 'https://picsum.photos/200/300' },
          { name: 'French Toast', description: 'Classic French toast topped with powdered sugar', price: '$9', image: 'https://picsum.photos/200/300' },
          { name: 'Omelette', description: 'Fluffy omelette with your choice of fillings', price: '$10', image: 'https://picsum.photos/200/300' },
          ],
        lunch: [
          { name: 'Caesar Salad', description: 'Romaine lettuce with Caesar dressing and croutons', price: '$12', image: 'https://picsum.photos/200/300' },
          { name: 'Grilled Chicken Sandwich', description: 'Grilled chicken breast with lettuce, tomato, and mayo', price: '$14', image: 'https://picsum.photos/200/300' },
          { name: 'Vegetable Wrap', description: 'Assorted vegetables wrapped in a tortilla', price: '$11', image: 'https://picsum.photos/200/300' },
          { name: 'Burger', description: 'Juicy beef patty with cheese, lettuce, and tomato', price: '$13', image: 'https://picsum.photos/200/300' },
          { name: 'Club Sandwich', description: 'Triple-decker sandwich with turkey, bacon, lettuce, and tomato', price: '$15', image: 'https://picsum.photos/200/300' },
          { name: 'Soup of the Day', description: 'Homemade soup made with fresh ingredients', price: '$10', image: 'https://picsum.photos/200/300' }
        ],
        dinner: [
          { name: 'Steak', description: 'Grilled sirloin steak served with mashed potatoes and vegetables', price: '$25', image: 'https://picsum.photos/200/300' },
          { name: 'Salmon', description: 'Pan-seared salmon fillet with lemon butter sauce', price: '$22', image: 'https://picsum.photos/200/300' },
          { name: 'Pasta Primavera', description: 'Pasta with assorted vegetables in a creamy sauce', price: '$18', image: 'https://picsum.photos/200/300' },
          { name: 'Roast Chicken', description: 'Herb-roasted chicken served with roasted potatoes', price: '$20', image: 'https://picsum.photos/200/300' },
          { name: 'Shrimp Scampi', description: 'Shrimp sautéed in garlic butter and white wine sauce', price: '$24', image: 'https://picsum.photos/200/300' },
          { name: 'Vegetarian Stir-Fry', description: 'Assorted vegetables stir-fried in a savory sauce', price: '$16', image: 'https://picsum.photos/200/300' }
        ]
      },
      itemsPerPage: 6,
      currentPage: 0
    };
  },
  computed: {
    selectedMenuItems() {
      return this.menus[this.selectedMenu];
    },
    paginatedMenu() {
      const start = this.currentPage * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      const slicedItems = this.selectedMenuItems.slice(start, end);
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
      return Math.ceil(this.selectedMenuItems.length / this.itemsPerPage);
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

.menu-choice {
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.menu-choice button {
  background-color: transparent;
  color: black;
  border: 2px solid black;
  padding: 10px 20px;
  margin: 0 10px;
  cursor: pointer;
}

.menu-choice button.active {
  background-color: black;
  color: white;
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
</style>
