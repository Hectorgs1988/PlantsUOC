<template>
    <div class="plant-list">
      <PlantItem
        v-for="plant in plantList"
        :key="plant.id"
        :plantInfo="plant"
        @delete-plant="handleDeletePlant"
      />
    </div>
  </template>

<script setup>
import PlantItem from './PlantItem.vue';
import { defineProps, defineEmits } from 'vue';

//Usar defineProps para definir propiedades y obtener beneficios
const props = defineProps({
  plantList: {
    type:Array,
    required: true
  }
});

const emit = defineEmits(['delete-plant']);

//Funcion para manejar el evento delete-plant y remitirlo
const handleDeletePlant = (id) => {
  emit('delete-plant', id);
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
  