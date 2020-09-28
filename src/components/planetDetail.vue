<template>
  <div class="modal">
      <div class="modal-layer" v-on:click="$emit('close')"></div>
      <div class="modal-container">
          <button class="close-button" v-on:click="$emit('close')">X</button>
          <div class="card">
              <div class="card__image">
                  <img src="https://www.sciencemag.org/sites/default/files/styles/article_main_large/public/planet_1280p.jpg?itok=07tueTM9" :alt="this.planetData.name" />
              </div>
              <div class="card__title">
                  <span>Planet details</span>
                  <h2>{{this.planetData.name}}</h2>
              </div>
              <div class="card__info">
                  <div class="card__info-item">
                      <p class="label">Climate</p>
                      <p class="data">{{this.planetData.climate}}</p>
                  </div>
                  <div class="card__info-item">
                      <p class="label">Created</p>
                      <p class="data">{{this.date}}</p>
                  </div>
                  <div class="card__info-item">
                      <p class="label">Diameter</p>
                      <p class="data">{{this.planetData.diameter}}</p>
                  </div>
                  <div class="card__info-item">
                      <p class="label">Gravity</p>
                      <p class="data">{{this.planetData.gravity}}</p>
                  </div>
                  <div class="card__info-item">
                      <p class="label">Orbital period</p>
                      <p class="data">{{this.planetData.orbital_period}}</p>
                  </div>
                  <div class="card__info-item">
                      <p class="label">Population</p>
                      <p class="data">{{this.planetData.population}}</p>
                  </div>
                  <div class="card__info-item">
                      <p class="label">Rotation period</p>
                      <p class="data">{{this.planetData.rotation_period}}</p>
                  </div>
                  <div class="card__info-item">
                      <p class="label">Terrain</p>
                      <p class="data">{{this.planetData.terrain}}</p>
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
    name: 'planetDetail',
    props: {
        planetDetail: {
            type: String,
            default: () => ({}),
        },
    },
    data(){
        return{
           planetData: [],
           date: '',
           show: false,
        }
    },
    methods: {
        getData(url){
           fetch(url)
            .then(response => response.json())
            .then(json => {
                this.planetData = json
                this.date = moment(this.planetData.created).format("MMM Do YY")
            })
            .catch(error => {
                this.errorMessage = error;
                console.error("There was an error!", error);
            }); 
        },
    },
    mounted() {
        this.getData(this.planetDetail)
    }
}
</script>
<style lang="scss">
  @import '../assets/styles/detail.scss';
</style>