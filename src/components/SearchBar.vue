<template>
    <div class="search-bar">
      <input type="text" placeholder="Search plant" v-model="searchQuery" @input="onSearch" />
      <button class="clear" @click="clearSearch">Clear</button>
      <button class="add" @click="addPlant">Add plant</button>
    </div>
</template>
  

<script setup>
import { ref, defineEmits } from 'vue';

//Definir los eventos que se pueden emitir en este componente
const emit = defineEmits(['search', 'add-plant']);

//Utiliza ref para hacer reactivos los datos que necesitan ser observados por Vue
const searchQuery = ref('');

function onSearch(){
  emit('search', searchQuery.value);
}

function clearSearch(){
  searchQuery.value = '';
  onSearch();
}

function addPlant(){
  emit('add-plant');
}
</script>

  
<style scoped>
.search-bar {
display: flex;
justify-content: space-between;
align-items: center;
padding: 10px 20px;
background-color: #f5f5f5;
border-bottom: 1px solid #e5e5e5;
}
.search-bar input {
width: 100%;
padding: 10px;
border: 1px solid #e5e5e5;
border-radius: 4px;
outline: none;
}
.search-bar button {
padding: 10px;
border: 1px solid #e5e5e5;
border-radius: 4px;
outline: none;
cursor: pointer;
min-width: 100px;
}
.search-bar .clear {
margin-right: 10px;
}
.search-bar .add {
background-color: #007bff;
color: #fff;
}
</style>
  