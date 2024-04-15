<template>
    <div class="plant-item" :class="{ 'plant-item--favorite': plantInfo.favorite }">
      <div class="plant-item__id" :data-id="plantInfo.id"></div>

      <button class="plant-item__delete">
        <img :src="deleteIcon" alt="Delete plant">
      </button>

      <div class="plant-item__image">
        <img :src="`/public/${plantInfo.image}`" alt="Plant image" />
      </div>

      <div class="plant-item__info">
        <h2 class="plant-item__title">{{ plantInfo.name }}</h2>
        <div class="plant-item__data">
          <p class="plant-item__date">Date: {{ plantInfo.date }}</p>
          <p class="plant-item__family">Family: {{ plantInfo.family }}</p>
          <p class="plant-item__genus">Genus: {{ plantInfo.genus }}</p>
          <p class="plant-item__species">Species: {{ plantInfo.species }}</p>
        </div>

        <p class="plant-item__description">{{ plantInfo.description }}</p>
        <p class="plant-item__personal-note">{{ plantInfo.personalNote }}</p>

        <div class="plant-item__extra">
          <p class="plant-item__rating">{{ plantInfo.rating }} / 5</p>
          <p class="plant-item__labels">
            <span v-for="label in plantInfo.labels" :key="label">{{ label }}</span>
          </p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import deleteIcon from '@/assets/images/delete-button.svg';

  export default {
    name: 'PlantItem',
    props: {
      plantInfo: {
        type: Object,
        required: true
      },
    },

    
    //TODO: Revisar porque no me funciona esto!! 
    //Alternativa que voy a usar es meter las imagenes en la carpeta public
/*
    computed: {
    imageUrl() {
      return require(`@/assets/images/${this.plantInfo.image}`);
    },
  },
    */
  
    data() {
      return{
        deleteIcon: deleteIcon,
      };
    },
  };
  </script>
  
<style scoped>
.plant-item {
display: flex;
padding: 1rem;
border: 1px solid #ccc;
border-radius: 0.5rem;
margin-bottom: 1rem;
position: relative;
}
.plant-item--favourite {
background-color: #f0f0f0;
}
.plant-item__delete {
background-color: transparent;
border: none;
padding: 10px;
cursor: pointer;
position: absolute;
top: 0;
right: 0;
}
.plant-item__delete img {
width: 20px;
}
.plant-item__image {
margin-right: 1rem;
}
.plant-item__image img {
width: 200px;
height: 200px;
object-fit: cover;
border-radius: 0.5rem;
}
.plant-item__info {
    flex: 1;
}
.plant-item__title {
margin: 0;
font-size: 1.5rem;
margin-bottom: 1rem;
border-bottom: 1px solid #ccc;
}
.plant-item__description {
margin: 0 0 1rem;
}
.plant-item__data {
margin-bottom: 1rem;
border-bottom: 1px solid #ccc;
padding-bottom: 1rem;
}
.plant-item__data p {
margin: 0;
}
.plant-item__personal-note {
margin: 0 0 1rem;
}
.plant-item__extra {
display: flex;
justify-content: space-between;
}
.plant-item__rating {
margin: 0;
}
.plant-item__labels {
margin: 0;
}
.plant-item__labels span {
margin-right: 0.5rem;
padding: 0.25rem 0.5rem;
border: 1px solid #ccc;
border-radius: 0.25rem;
background-color: #f0f0f0;
}
</style>
  