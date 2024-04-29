<template>
    <div class="plant-list">
      <PlantItem
        v-for="plant in plantList"
        :key="plant.id"
        :plantInfo="plant"
      />
    </div>
  </template>
  
  <script>
  //TODO: Modificar por Composition.API
  import PlantItem from './PlantItem.vue';
  
  export default {
    name: 'PlantList',
    components: {
      PlantItem
    },
    props: {
      plantList: {
        type: Array,
        required: true
      },
    },
  };
  </script>
  
  <style scoped>
  .plant-list {
    padding: 20px;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 20px;
  }
  </style>
  