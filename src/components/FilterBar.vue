<template>
    <div class="filter-bar">
      <!-- To search for... -->
      <div class="filter-bar__select">
        <label for="sortBy">Sort by:</label>
        <select id="sortBy" v-model="sortBy">
          <option value="name">Name</option>
          <option value="date">Date</option>
          <option value="family">Family</option>
          <option value="rating">Rating</option>
        </select>
      </div>
  
      <!-- Order Ascending / Descending -->
      <div class="filter-bar__select">
        <label for="order">Sort by:</label>
        <select id="order" v-model="order">
          <option value="asc">Ascending</option>
          <option value="desc">Descending</option>
        </select>
      </div>
  
      <!-- Favorites filter -->
      <div class="filter-bar__check">
        <input type="checkbox" id="favoriteFilter" v-model="showFavorites">
        <label for="favoriteFilter">Favorite</label>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'FilterBar',
    data() {
      return {
        sortBy: 'name',
        order: 'asc',
        showFavorites: false,
      };
    },
    watch: {
      sortBy(newVal) {
        this.emitFilterUpdate();
      },
      order(newVal) {
        this.emitFilterUpdate();
      },
      showFavorites(newVal) {
        this.emitFilterUpdate();
      },
    },
    methods: {
      emitFilterUpdate() {
        this.$emit('update-filter', {
          sortBy: this.sortBy,
          order: this.order,
          showFavorites: this.showFavorites,
        });
      },
    },
  };
  </script>
  
  <style scoped>
  .filter-bar {
    display: flex;
    align-items: center;
    padding: 20px;
    background-color: #f5f5f5;
    border-bottom: 1px solid #ccc;
  }
  .filter-bar__select {
    display: flex;
    align-items: center;
    margin-right: 20px;
  }
  .filter-bar__select label {
    margin-right: 10px;
  }
  .filter-bar__select select {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    outline: none;
  }
  .filter-bar__check {
    display: flex;
    align-items: center;
  }
  .filter-bar__check input {
    margin-right: 10px;
  }
  .filter-bar__check label {
    cursor: pointer;
  }
  </style>
  