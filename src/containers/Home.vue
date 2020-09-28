<template>
  <div class="planets">
    <header class="planets-header">
        <img src="https://www.freepnglogos.com/uploads/star-wars-logo-1.png" class="sw-logo" alt="Star Wars" />
        <div class="subHeader">
            <h1 class="main-title">Planet List</h1>
            
            <div class="filters">
                <input type="text" v-model="search" placeholder="Search planet" class="search" />
                <p>Sort by</p>
                <button v-on:click="orderPlanets()">name</button>
                <button v-on:click="orderPopulation()">population</button>
            </div>
        </div>
    </header>
    <div v-if="this.generalList.length" class="planet-list">
        <planet-list-item v-for="(planet) in this.transformedList" :key="planet.name" :planet-data={planet} />
    </div>
    <div v-else class="error">
        <img class="error__image" src="https://www.freeiconspng.com/uploads/star-wars-png-file-31.png" alt="Space Ship" />
        <p class="error__message">We're having problems showing the planets</p>
    </div>
  </div>
</template>

<script>
import PlanetListItem from '../components/planetListItem'

export default {
    name: 'Home',
    components: {
        PlanetListItem,
    },
    data(){
        return {
            //Initialize 
            url: "https://swapi.dev/api/planets/",
            //Initialize list variables
            planetList: [],
            generalList: [],
            next: '',
            loadMore: false,
            key: 'index',
            search: '',
        }
    },
    methods: {
        getData(url){
           fetch(url)
            .then(response => response.json())
            .then(json => {
                this.planetList = json.results
                if(json.next){
                    this.next = json.next.replace('http', 'https')
                }else {
                    this.next = ''
                }
                
                //Loop through planetList and add individual items to generalList
               for(const property in this.planetList){
                   this.generalList.push(this.planetList[property])
               }
               this.loadMore = true
            })
            .catch(error => {
                this.errorMessage = error;
                console.error("There was an error!", error);
            }); 
        },
        scroll() {
            //Add next 10 planets when the user reach the end of the page
            window.onscroll = () => {
                let bottomOfWindow = (window.innerHeight + window.scrollY) >= document.body.offsetHeight
                if (bottomOfWindow) {
                    //If is there more data
                    if(this.next && this.loadMore){
                        this.getData(this.next) 
                        this.loadMore = false
                    }
                }
            } 
        },
        orderPopulation(){
            this.generalList = this.generalList.sort(function (a, b) { 
                if (a.population < b.population) {
                    return -1;
                }
                if (a.population > b.population) {
                    return 1;
                }
                    return 0;
            });
        },
        orderPlanets(){
            this.generalList = this.generalList.sort(function(a, b) {
                var nameA = a.name.toUpperCase(); // ignore upper and lowercase
                var nameB = b.name.toUpperCase(); // ignore upper and lowercase
                if (nameA < nameB) {
                return -1;
                }
                if (nameA > nameB) {
                return 1;
                }
                // names must be equal
                return 0;
            });
        }

    },
    computed: {
        transformedList(){
            if(this.search){
                return this.generalList.filter((planet)=>{
                    return this.search.toUpperCase().split(' ').every(v => planet.name.toUpperCase().includes(v))
                })
            }else{
                return this.generalList;
            }
        } 
    },
    mounted() {
        // Get the data from api
        this.getData(this.url);
        //Initialize scroll function
        this.scroll()
    },
}
</script>
<style lang="scss">
  @import '../assets/styles/home.scss';
</style>