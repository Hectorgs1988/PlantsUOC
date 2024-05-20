<template>
  <div class="plant-form">
    <form class="plant-form__form" @submit.prevent="submitForm">
      <div class="plant-form__form-row">
        <div class="plant-form__form-group">
          <label for="name">Nombre</label>
          <input id="name" v-model="plant.name" type="text" required>

          <label for="image">URL de Imagen</label>
          <input id="image" v-model="plant.image" type="text" required>

          <label for="family">Familia</label>
          <input id="family" v-model="plant.family" type="text" required>

          <label for="species">Especie</label>
          <input id="species" v-model="plant.species" type="text" required>

          <label for="favorite">Favorito</label>
          <input id="favorite" v-model="plant.favorite" type="checkbox">
        </div>
        <div class="plant-form__form-group">
          <label for="personalNote">Nota Personal</label>
          <input id="personalNote" v-model="plant.personalNote" type="text">
        </div>
      </div>
      <div class="plant-form__form-row">
        <div class="plant-form__form-group">
          <label for="description">Descripción</label>
          <input id="description" v-model="plant.description" type="text" required>

          <label for="date">Fecha</label>
          <input id="date" v-model="plant.date" type="date" required>

          <label for="genus">Género</label>
          <input id="genus" v-model="plant.genus" type="text" required>

          <label for="labels">Etiquetas</label>
          <input id="labels" v-model="plant.labels" type="text">
        </div>
        <div class="plant-form__form-group">
          <label for="rating">Calificación</label>
          <input id="rating" v-model="plant.rating" type="range" min="0" max="5">
          <span>{{ plant.rating }}</span>
        </div>
      </div>
      <div class="plant-form__form-group plant-form__form-group--actions">
        <button type="submit" class="plant-form__submit">Agregar Planta</button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';
import { v4 as uuidv4 } from 'uuid';

const emit = defineEmits(['submit-plant']);

const plant = ref({
  id: '',
  name: '',
  description: '',
  image: '',  // Cambiado de imageURL a image
  date: '',
  family: '',
  genus: '',
  species: '',
  labels: '',
  favorite: false,
  rating: 0,
  personalNote: '',
});

const submitForm = () => {
  if (!plant.value.name || !plant.value.description || !plant.value.image || !plant.value.date) {
    alert("Por favor, complete todos los campos requeridos.");
    return;
  }

  const labelsArray = plant.value.labels.split(',').map(label => label.trim());
  const newPlant = {
    ...plant.value,
    id: uuidv4(),
    labels: labelsArray
  };

  console.log('Nueva planta:', newPlant); // Para depuración
  emit('submit-plant', newPlant);
  resetForm();
};

const resetForm = () => {
  for (const key in plant.value) {
    if (typeof plant.value[key] === 'boolean') {
      plant.value[key] = false;
    } else {
      plant.value[key] = '';
    }
  }
  plant.value.rating = 0;
};
</script>

<style scoped>
  .plant-form {
    padding: 1rem;
  }
  
  .plant-form__form {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
  
  @media (max-width: 768px) {
    .plant-form__form {
      grid-template-columns: 1fr;
    }
  }
  
  .plant-form__form-group {
    display: flex;
    flex-direction: column;
  }
  
  .plant-form__form-group label {
    margin-bottom: 0.5rem;
  }
  
  .plant-form__form-group input,
  .plant-form__form-group textarea {
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 0.25rem;
  }
  
  .plant-form__form-group input[type="checkbox"],
  .plant-form__form-group input[type="range"] {
    width: auto;
  }
  
  .plant-form__form-group--actions {
    grid-column: 1 / -1;
  }
  
  .plant-form__submit {
    padding: 0.5rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 0.25rem;
    cursor: pointer;
  }
</style>
