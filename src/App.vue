<template>
  <div>
    <header class="header">
      <img src="@/assets/images/uoc-logo.png" alt="UOC Logo" class="logo" />
      <h1 class="title">Plants Book</h1>
    </header>
    <div class="content">
      <SearchBar @search="handleSearch" @add-plant="handleAddPlant" />
      <FilterBar @update-filter="handleFilterUpdate" />
      <main class="main">
        <PlantList :plantList="filteredPlantList" @delete-plant="handleDeletePlant" />
      </main>
      
      <!-- Modal to add a new plant -->
      <ModalLayer v-if="isModalVisible" @close="closeModal">
        <template #header>
          <h2>Add new plant</h2>
        </template>
        <template #body>
          <!-- Component PlantForm to add a new plant -->
          <PlantForm @submit-plant="addNewPlant" />
        </template>
      </ModalLayer>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import PlantList from './components/PlantList.vue';
import SearchBar from './components/SearchBar.vue';
import FilterBar from './components/FilterBar.vue';
import ModalLayer from './components/ModalLayer.vue';
import PlantForm from './components/PlantForm.vue';
import axios from 'axios';

// Initial state
const plantList = ref([]);
const filteredPlantList = ref([]);
const currentFilterCriteria = reactive({
  query: '',
  sortBy: 'name',
  order: 'asc',
  showFavorites: false,
});
const isModalVisible = ref(false);

// Cargar datos iniciales de la API 
//TODO:review HTTP image
onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:3000/garden');
    console.log('Response from API:', response.data);
    plantList.value = Array.isArray(response.data.garden) ? response.data.garden : [];
    applyFilters();
  } catch (error) {
    console.error('Error al cargar plantas:', error);
  }
});

const handleSearch = (query) => {
  currentFilterCriteria.query = query;
  applyFilters();
};

const handleFilterUpdate = (criteria) => {
  Object.assign(currentFilterCriteria, criteria);
  applyFilters();
};

const applyFilters = () => {
  if (!Array.isArray(plantList.value)) {
    console.error('plantList.value is not an array:', plantList.value);
    return;
  }

  let result = plantList.value.slice();

  if (currentFilterCriteria.showFavorites) {
    result = result.filter(plant => plant.favorite);
  }

  if (currentFilterCriteria.query) {
    result = result.filter(plant =>
      plant.name.toLowerCase().includes(currentFilterCriteria.query.toLowerCase())
    );
  }

  result.sort((a, b) => {
    let comparison = 0;
    if (a[currentFilterCriteria.sortBy] < b[currentFilterCriteria.sortBy]) comparison = -1;
    if (a[currentFilterCriteria.sortBy] > b[currentFilterCriteria.sortBy]) comparison = 1;
    return currentFilterCriteria.order === 'asc' ? comparison : -comparison;
  });

  filteredPlantList.value = result;
};

const handleAddPlant = () => {
  isModalVisible.value = true;
};

const closeModal = () => {
  isModalVisible.value = false;
};

//Add a new plant
const addNewPlant = async (newPlantData) => {
  try {
    //Required Fieds
    const requiredFields = ['id', 'name', 'description', 'image', 'date']; 
    const missingFields = requiredFields.filter(field => !newPlantData[field]);

    //TODO: depuracion
    // if (missingFields.length > 0) {
    //   console.error(`Faltan los siguientes campos requeridos: ${missingFields.join(', ')}`);
    //   alert(`Por favor, complete todos los campos requeridos: ${missingFields.join(', ')}`);
    //   return;
    // }

    //Rating to numbert
    newPlantData.rating = Number(newPlantData.rating);

    const postResponse = await axios.post('http://localhost:3000/plant', newPlantData);
  
    if (postResponse.data && !postResponse.data.error) {
      const response = await axios.get('http://localhost:3000/garden');
      console.log('Lista de plantas actualizada:', response.data);
      plantList.value = Array.isArray(response.data.garden) ? response.data.garden : [];
      applyFilters();
      closeModal();
    } else {
      console.error('Error al añadir la planta:', postResponse.data.message);
    }
  } catch (error) {
    console.error('Error al añadir la planta:', error);
  }
};

// Delete plant
const handleDeletePlant = async (id) => {
  try {
    const deleteUrl = `http://localhost:3000/plant`;
    console.log(`URL de eliminación: ${deleteUrl}`);

    const deleteResponse = await axios.delete(deleteUrl, {
      data: { id }
    });
  
    const response = await axios.get('http://localhost:3000/garden');
    console.log('Lista de plantas actualizada:', response.data);
    plantList.value = Array.isArray(response.data.garden) ? response.data.garden : [];
    applyFilters();
  } catch (error) {
    console.error('Error al eliminar la planta:', error);
  }
};
</script>

<style scoped>
body {
padding: 0;
margin: 0;
font-family: "Avenir", Helvetica, Arial, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
text-align: center;
color: #2c3e50;
}
.header {
display: flex;
justify-content: space-between;
align-items: center;
padding: 0 20px;
background-color: #f5f5f5;
border-bottom: 1px solid #e5e5e5;
}
.header__left {
display: flex;
align-items: center;
}
.logo {
height: 40px;
margin-right: 10px;
}
.title {
font-size: 24px;
font-weight: 400;
}
</style>
