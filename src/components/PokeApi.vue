<template>
  <div class="body">
      <div id="page">  
        <div class="container">

          <header>
            <!-- imagen de titulo 1 -->
            <div class="col-lg-12 text-center">
              <img class="imgtitulo" src="https://fontmeme.com/permalink/201106/cf773afac5fcf6dac24f59f19ca387b2.png" alt=""> 
              <hr>
            </div>
          </header>
          
          <section>
            <div class="row">          
              <div class="poke-container result card-body">

                <!-- buscador -->
                <div class="searchBar">
                  <form @submit.prevent="mostrarPoke">
                    <div class="search-wrap">
                      <!-- <spam class="text-dark">Ingresa el nombre o numero del pokemon a elegir: </spam> -->
                      <input type="text" placeholder="Ej: Charizard o 6" v-model="pokemon">
                      <button class="btn text-dark" type="submit"> <i class="fab fa-searchengin fa-2x"> </i> </button>
                    </div>
                  </form>
                </div>

                <!-- imagen de titulo 2 -->
                <img class="imgtitulo2" src="https://fontmeme.com/permalink/201106/d7efdbafb7979fdabb64ae0049a14ef0.png" alt="">

                <!-- contendor de datos -->
                <div id="pokeOutput">
                  
                  <!-- color de fondo -->
                  <div class="top-container">

                    <!-- almacena los botones dentro del contenedor -->
                    <div class="image-container text-white border-0">

                      <!-- menu de navegacion -->
                      <!-- <div id="pic-nav">
                        <button type="button" class="changeBtn" aria-label="Girar izquierda"> <i class="fas fa-angle-left"></i> </button>
                        <button type="button" id="defaultBtn" class="bigBtn">Normal</button>
                        <button type="button" id="shinyBtn" class="bigBtn">Shiny</button>
                        <button type="button" class="changeBtn" aria-label="Girar derecha"> <i class="fas fa-angle-right"></i> </button>
                      </div> -->

                      <!-- nombre y numero del pokemon -->
                      <div class="p-2">
                        <!-- nombre del pokemon-->
                        <span class="name poke-info">{{getNombre}}</span>
                        <!-- numero del pokemon -->
                        <span class="idNum poke-info"> #{{getId}}</span>
                      </div>

                      <!-- peso y altura del pokemon -->
                      <div class="p-2" id="types">
                        <span class="poke-info"><span class="weight">Peso:</span> {{getPeso}} Kgs. </span>
                        <span class="poke-info"><span class="height">Altura:</span> {{getAltura}} M.</span>
                      </div>

                      <!-- imagen del pokemon -->
                      <img :src="getImg" id="pokeImage" v-if="mostar">
                      
                      <!-- mega Evolusiones -->
                      <!-- <div class="text-center">
                        <button type="button" id="megaX" class="BtnMega">Charizard Mega-X</button>
                        <button type="button" id="megaY" class="BtnMega">Charizard Mega-Y</button>
                      </div> -->

                      <!-- datos del tipo de pokemon -->
                      <div class="pb-1 d-inline-block" id="types" v-for="(poke,index) in getTipos" :key="index">
                          <div class="pokeType poke-info" :class="[poke.type.name]">{{poke.type.name}}</div>
                      </div>

                    </div>

                    <!-- habilidades de los pokemon -->
                    <div class="row">

                      <!-- cambia color a la tabla -->
                      <div class="table-responsive col-12 col-sm-12 col-md-6 col-lg-6 col-xl-6">
                        <table class="table table-sm table-hover table-dark text-center">
                          <thead>
                              <tr class="bgTitulo">
                                <th scope="col">Habilidades</th>
                              </tr>
                          </thead>
                          <tbody id="habilitys">
                            <tr v-for="(habilidad, index) in getHablidades" :key="index">
                              <th scope="col">{{habilidad.ability.name}}</th>
                            </tr>
                          </tbody>

                        </table>                                                         
                      </div>

                      <!-- grafico con las estaditicas del pokemon -->
                      <div class="col-12 col-sm-12 col-md-6 col-lg-6 col-xl-6" >
                        <div id="chartContainer"></div>
                      </div>

                    </div>              
                  </div>
                </div> 
              </div>
            </div>

          </section>

        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeApi',
  data() {
    return {
      pokemon: "",
      listaPokemon: {},
      mostar: false,
    };
  },
  created() {
    fetch('https://pokeapi.co/api/v2/pokemon/pikachu')
    .then(resp => resp.json())
    .then((data) => {
      this.listaPokemon = data;
    });
  },
  methods: {
    mostrarPoke(){
      fetch('https://pokeapi.co/api/v2/pokemon/' + this.pokemon)
      .then(resp => resp.json())
      .then((data) => {
        this.listaPokemon = data;
        this.pokemon = "";
      })
      .catch((error) => {console.log(error)
        alert("El nombre o número no es correcto");
      })
    }
  },
  computed:{
    getId(){
      return this.listaPokemon.id
    },
    getNombre(){
      return this.listaPokemon.name
    },
    getImg(){
      return this.listaPokemon.sprites.front_default
    },
    getTipos(){
      // console.log(this.listaPokemon.types[0].type.name)
      return this.listaPokemon.types
    },
    getHablidades(){
      return this.listaPokemon.abilities
    },
    getPeso(){
      return this.listaPokemon.weight/10
    },
    getAltura(){
      return this.listaPokemon.height/10
    }
  },

  mounted() {
    if (this.listaPokemon) {      
      setTimeout(() => {
        this.mostar = true
      }, 500);
    }
  } 
}

</script>

// stilos
<style scoped>
  @charset "UTF-8";
  /* pagina  principal */
  #page {
    width: 100%;
    min-height: 100vh;
    overflow: hidden;
  }

  .body {
    margin: 0;
    display: flex;
    justify-content: center;
    background-image: url("../assets/img/fondo4.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
  }

  /* container princiapl */
  .poke-container {
    width: 100%;
    background-image: url("../assets/img/fondo2.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    justify-content: center;
    align-content: center;
    align-items: center;
  }

  /* mantiene los botones en linea en tamaños pequeños*/
  .image-container {
    white-space: nowrap;
    font-size: 1.2rem;
  }

  /* propiedades panel de botones centrales*/
  button {
    display: inline-block;
    margin: 0;
    position: relative;
  }

  button:active {
    background-color: #8b8fa5;
  }

  .bigBtn {
    height: 40px;
    width: 39%;
    font-weight: 700;
    top: -2px;
  }

  /* propiedades panel de botones laterales*/
  .changeBtn {
    height: 39px;
    width: 10%;
    font-size: 1.5rem;
  }

  .BtnMega {
    height: 40px;
    width: 39%;
    font-weight: 700;
    top: -2px;
  }

  /* barra de busqueda */
  .searchBar {
    padding: 20px 0;
    text-align: center;
    /* background-color: #192CEF; */
  }

  /* input del buscador */
  input[type=text] {
    height: 40px;
    padding-left: 10px;
    display: inline-block;
    vertical-align: middle;
  }

  /* donde se muetran los datos del pokemon */
  #pokeOutput {
    width: 100%;
    padding: 10px;
  }

  /* imagen del pokemon */
  #pokeImage {
    display: block;
    margin: 10px auto;
    width: 200px;
    position: relative;
  }

  /* texto nombre pokemon  */
  .name {
    display: block;
    text-transform: capitalize;
    font-size: 1.6rem;
  }

  /* tamaño que muestra el tipo de pokemon  */
  .poke-info {
    display: inline-block;
    margin-right: 2px;
  }

  /* tabla habilidades pokemon*/
  table {
    border-collapse: collapse;
    width: 100%;
    font-size: 1.2rem;
  }

  th,
  td {
    color: #f1f6ff;
    border: 1px solid #000000;
    text-align: center;
    /* padding: 8px; */
  }

  .bgTitulo {
    background-color: #7b0b06;
  }

  td {
    background-color: #7a706f;
  }

  #chartContainer {
    height: 370px;
    width: 100%;
  }

  /* colores segun el tipo/s del pokemon */
  .pokeType {
    padding: 3px;
    color: #fff;
    border-radius: 5px;
    text-transform: capitalize;
    margin-left: 5px;
    font-weight: 700;
  }

  .normal {
    background-color: #a8a878;
  }

  .grass {
    background-color: #78c850;
  }

  .ground {
    background-color: #e0c068;
  }

  .fighting {
    background-color: #c03028;
  }

  .rock {
    background-color: #b8a038;
  }

  .steel {
    background-color: #b8b8d0;
  }

  .fire {
    background-color: #f08030;
  }

  .electric {
    background-color: #f8d030;
  }

  .flying {
    background-color: #a890f0;
  }

  .psychic {
    background-color: #f85888;
  }

  .bug {
    background-color: #a8b820;
  }

  .dragon {
    background-color: #7038f8;
  }

  .water {
    background-color: #6890f0;
  }

  .ice {
    background-color: #98d8d8;
  }

  .poison {
    background-color: #a040a0;
  }

  .dark {
    background-color: #705848;
  }

  .ghost {
    background-color: #705898;
  }

  .fairy {
    background-color: #ffaec9;
  }

  @media (max-width: 575.98px) {
    .imgtitulo {
      width: 230px;
    }

    .imgtitulo2 {
      width: 280px;
    }
  }

  /*# sourceMappingURL=main.css.map */

</style>
