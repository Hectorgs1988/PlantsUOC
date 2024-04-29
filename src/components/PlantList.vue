<template>
    <div class="plant-list">
      <PlantItem
        v-for="plant in plantList"
        :key="plant.id"
        :plantInfo="plant"
      />
    </div>
  </template>
  
  <!--
  <script>
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
  -->

<script setup>
import PlantItem from './PlantItem.vue';
import { defineComponent, defineProps } from 'vue';

//Usar defineProps para definir propiedades y obtener beneficios
const props = defineProps({
  plantList: {
    type:Array,
    required: true
  }
});

//Registrar el componente PlantItem para uso en este componente
defineComponent({
  components: {
    PlantItem
  }
});
</script>

  
  <style scoped>
  .plant-list {
    padding: 20px;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 20px;
  }
  </style>
  