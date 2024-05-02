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
   rating: 4,
   image: 'fiddle-leaf-fig-plant-pot.jpg'
 },

 {
   id: 3,
   name: 'Snake Plant',
   description: 'Lorem ipsum dolor sit amet consectetur adipiscing elit tortor in, feugiat tempus nec eu phasellus nam varius montes, nisi imperdiet bibendum faucibus libero pretium hac nullam. Sociosqu iaculis etiam ridiculus viverra fringilla pulvinar, ac praesent leo egestas purus orci quisque, convallis rhoncus primis duis cursus. Tempor arcu integer dis ligula et luctus pellentesque penatibus, rutrum risus ornare sapien taciti est.',
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
   description: 'Lorem ipsum dolor sit amet consectetur adipiscing elit curabitur, dictum natoque posuere lacinia primis ridiculus quam est netus, gravida egestas dictumst porttitor class aliquam hac quis, et fringilla cras imperdiet dui eu etiam. Ad ut fames sem suscipit vel mus, diam facilisi lacus dignissim hendrerit nibh, tincidunt pulvinar volutpat proin cubilia.',
   date: '2021-06-01',
   family: 'Araceae',
   genus: 'Monstera',
   species: 'Monstera deliciosa',
   labels: ['Tropical', 'houseplant', 'low maintenace'],
   favorite: false,
   rating: 3,
   image: 'pothos-plant-pot-bench.jpg'
 },

 {
   id: 5,
   name: 'Ficus Europeo',
   description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam accumsan tellus et felis vehicula, vitae porta ex convallis. Phasellus ut euismod leo. Fusce vitae pulvinar diam, ac egestas quam. Phasellus feugiat orci eget quam maximus porttitor. Sed sed consequat dui. Suspendisse commodo commodo odio, at congue massa blandit nec. Donec dignissim eu orci in molestie. Nunc in metus orci. Quisque vehicula neque a nunc accumsan commodo. Sed nec rhoncus tellus. Pellentesque vulputate sagittis massa ut mollis. In ullamcorper varius tortor at euismod. Phasellus feugiat ante sit amet mi pretium lobortis nec eget elit. Sed nec nisl in quam facilisis placerat.',
   date: '2021-06-01',
   family: 'Araceae',
   genus: 'Monstera',
   species: 'Monstera deliciosa',
   labels: ['Tropical', 'houseplant', 'low maintenace'],
   favorite: false,
   rating: 4,
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


const handleDeletePlant = (id) => {
  plantList.value = plantList.value.filter(plant => plant.id !== id);
};

const closeModal = () => {
  isModalVisible.value = false;
};


const addNewPlant = (newPlantData) => {
  const newPlantWithId = { ...newPlantData, id: Date.now() };
  plantList.value.push(newPlantWithId);
  applyFilters(); 
  closeModal();
  resetFormState(); 
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
