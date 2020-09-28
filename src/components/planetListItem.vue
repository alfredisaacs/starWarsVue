<template>
  <div class="planet">
        <span class="planet__init">Planet</span>
        <div class="planet__title">{{this.singlePlanet.name}}</div>
        <div class="planet-info">
            <div class="planet-info__population"><span>Population:</span> {{this.singlePlanet.population}}</div>
            <div class="planet-info__terrain"><span>Terrain:</span> {{this.singlePlanet.terrain}}</div>
            <div class="planet-info__movies"><span>Films:</span> {{this.singlePlanet.films.length}}</div>
            <button class="planet-info__button" v-on:click="showDetail(singlePlanet.url)">More info</button>
        </div>
        <planet-detail v-if="this.show" v-on:close="this.close" :planet-detail="this.planetToShow" />
  </div>
</template>

<script>
import planetDetail from './planetDetail'
export default {
    name: 'planetListItem',
    props: {
        planetData: {
            type: Object,
            default: () => ({}),
        }
    },
    components: {
        planetDetail,
    },
    data(){
        return{
            singlePlanet: this.planetData.planet,
            show: false,
            planetToShow: '',
        }
    },
    methods: {
        showDetail(url){
            this.planetToShow = url
            this.show = true
        },
        close(){
            this.show = false
        }
    }
}
</script>
<style lang="scss">
  @import '../assets/styles/planet.scss';
</style>