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
        <PlantList :plantList="filteredPlantList" />
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
import { ref, watch, reactive } from 'vue'
import PlantList from './components/PlantList.vue';
import SearchBar from './components/SearchBar.vue';
import FilterBar from './components/FilterBar.vue';
import ModalLayer from './components/ModalLayer.vue';
import PlantForm from './components/PlantForm.vue';

//Generation of test plants
const plantList = ref([
  {
    id: 1,
    name: 'Monstera',
    description: 'The Monstera is a tropical plant native to Central America. It is known for its large, glossy leaves that have natural holes in them. The Monstera is a popular houseplant due to its unique appeareance and low maintenance requierements.',
    date: '2021-06-01',
    family: 'Araceae',
    genus: 'Monstera',
    species: 'Monstera deliciosa',
    labels: ['Tropical', 'houseplant', 'low maintenace'],
    favorite: true,
    rating: 5,
    image: 'monstera-deliciosa-plant-pot.jpg'

  },
  

  {
   id: 2,
   name: 'Fiddle Leaf Fig',
   description: 'The Fiddle is a houseplant known for its large, violin-shaped leaves. It is native to western Africa and is part of the Ficus genus. The Fiddle Leaf Fig is a favorite among interior designers due to its striking appearance and ability to grow indoors',
   date: '2021-06-01',
   family: 'Moraceae',
   genus: 'Ficus',
   species: 'Ficus lyrata',
   labels: ['houseplant', 'interior', 'low maintenace'],
   favorite: true,
   rating: 5,
   image: 'fiddle-leaf-fig-plant-pot.jpg'
 },

 {
   id: 3,
   name: 'Snake Plant',
   description: 'The Monstera is a tropical plant native to Central America. It is known for its large, glossy leaves that have natural holes in them. The Monstera is a popular houseplant due to its unique appeareance and low maintenance requierements.',
   date: '2021-06-01',
   family: 'Asparagaceae',
   genus: 'Monstera',
   species: 'Monstera deliciosa',
   labels: ['Tropical', 'houseplant', 'low maintenace'],
   favorite: true,
   rating: 5,
   image: 'still-life-with-indoor-plants.jpg'
 },

 {
   id: 4,
   name: 'Pothos',
   description: 'The Monstera is a tropical plant native to Central America. It is known for its large, glossy leaves that have natural holes in them. The Monstera is a popular houseplant due to its unique appeareance and low maintenance requierements.',
   date: '2021-06-01',
   family: 'Araceae',
   genus: 'Monstera',
   species: 'Monstera deliciosa',
   labels: ['Tropical', 'houseplant', 'low maintenace'],
   favorite: false,
   rating: 5,
   image: 'pothos-plant-pot-bench.jpg'
 },

 {
   id: 5,
   name: 'Ficus Europeo',
   description: 'The Monstera is a tropical plant native to Central America. It is known for its large, glossy leaves that have natural holes in them. The Monstera is a popular houseplant due to its unique appeareance and low maintenance requierements.',
   date: '2021-06-01',
   family: 'Araceae',
   genus: 'Monstera',
   species: 'Monstera deliciosa',
   labels: ['Tropical', 'houseplant', 'low maintenace'],
   favorite: false,
   rating: 5,
   image: 'botanical-leaves.jpg'
 },
]);

const filteredPlantList = ref([...plantList.value]);
const currentFilterCriteria = reactive({
  query: '',
  sortBy: 'name',
  order: 'asc',
  showFavorites: false,
});

const isModalVisible = ref(false);
const newPlant = reactive({
  name: '',
  family: '',
});

const handleSearch = (query) => {
  currentFilterCriteria.query = query;
  applyFilters();
};

const handleAddPlant = () => {
  isModalVisible.value = true;
};

const closeModal = () => {
  isModalVisible.value = false;
};

const addNewPlant = (newPlantData) => {
  plantList.value.push({ ...newPlant, id: Date.now() });
  applyFilters();
  closeModal();
  
  Object.keys(newPlant).forEach(key => newPlant[key] = '');
};

const handleFilterUpdate = (criteria) => {
  Object.assign(currentFilterCriteria, criteria);
  applyFilters();
};

const applyFilters = () => {
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

watch(plantList, () => {
  applyFilters();
});
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
