<template>
  <v-container >
    <h1 align='center'>Random Meal Generator App</h1>
    <v-row>
      <v-col>
        <v-card >
          <v-row>
            <v-col>
              <v-card-title align='center' class='display-2'>Grab a random meal</v-card-title>
              <v-btn  class='ml-4' @click='getMeal'>Random Meal</v-btn>
            </v-col>
            <v-col>
              <!-- strMealThumb -->
              <!-- strYoutube -->
                <v-img  max-width='20vw':src='imgSrc'></v-img>
            </v-col>
          </v-row>
          <v-card-text class='display-2' v-if='called'>{{info.meals[0].strMeal}}</v-card-text>

          <v-row v-if='called'>
            <v-col>
              <v-card-text class='headline'><strong>Ingredients</strong></v-card-text>
              <v-card-text v-for="i in numIngredients">{{i}}</v-card-text>
            </v-col>
            <v-col>
              <v-card-text class='headline'><strong>Amounts</strong></v-card-text>
              <v-card-text v-for="i in amIngredients">{{i}}</v-card-text>
            </v-col>
            <v-col>
              <v-card-text class='headline'><strong>Method</strong></v-card-text>
              <v-card-text >{{info.meals[0].strInstructions}}</v-card-text>
            </v-col>

          </v-row>

        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    info: null,
    errors: [],
    called: false,
    numIngredients: [],
    amIngredients: [],
    imgSrc: null,
  }),
  methods: {
    reset: function () {


    },
    getMeal: function () {
      this.called = true;
      axios.get("https://www.themealdb.com/api/json/v1/1/random.php")
        .then(response => {
          this.info = response.data;
          this.checkNumberIngredients();
          this.imgSrc = this.info.meals[0].strMealThumb;
        })
        .catch(e => {
          this.errors.push(e)
        })

         
    },
    checkNumberIngredients: function (){
      this.numIngredients = 0;
      var foodArray = [];
      var amountArray = [];
      const objArray = this.info.meals[0];

      for(const [key, value] of Object.entries(objArray)){
        if(key.match(/strIngredient/) && value != ""){
          foodArray.push(value);
        }
        if(key.match(/strMeasure/) && value != ""){
          amountArray.push(value);
        }

      }
      this.numIngredients = foodArray;
      this.amIngredients = amountArray;

    },
    getImage: function () {

    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
