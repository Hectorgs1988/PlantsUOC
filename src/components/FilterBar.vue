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
  
<script setup>
import {ref, watch} from 'vue';

const sortBy = ref('name');
const order = ref('asc');
const showFavorites = ref(false);

// Define los eventos que puede emitir este componente
const emit = defineEmits(['update-filter']);

const emitFilterUpdate = () =>{
  emit('update-filter',{
    sortBy:sortBy.value,
    order: order.value,
    showFavorites:showFavorites.value,
  });
};

watch([sortBy, order, showFavorites], ()=>{
  emitFilterUpdate();
});
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
  