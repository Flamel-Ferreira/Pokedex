<template>
  <div class="cartao">
    <h3>{{ this.propriedadesPokemon.id }} - {{ nomePokemonFormatado }}</h3>
    <div class="types">  
      <span :class="propriedadesPokemon.type1">
        {{ propriedadesPokemon.type1 }}
      </span> 
      <span v-if="this.propriedadesPokemon.mostrarTipoSecundario" :class="propriedadesPokemon.type2">
        {{ propriedadesPokemon.type2 }}
      </span>
    </div>
    <img v-bind:src="spriteAtual">
    <br>
    <button @click="mudarSprite">Mudar Sprite</button>
  </div>
</template>

<script>
import axios from 'axios';
  export default {
    created:function(){
      axios.get(this.url).then( res => {
        this.propriedadesPokemon.front = res.data.sprites.front_default;
        this.propriedadesPokemon.back = res.data.sprites.back_default;
        this.propriedadesPokemon.type1 = res.data.types[0].type.name;
        this.propriedadesPokemon.id = res.data.id;
        this.spriteAtual = this.propriedadesPokemon.front;

        if(res.data.types[1]){
          this.propriedadesPokemon.type2 = res.data.types[1].type.name;
          this.propriedadesPokemon.mostrarTipoSecundario = true;
        } else {
          this.propriedadesPokemon.mostrarTipoSecundario = false;
        }

        //EXPLICAÇÃO:
        /*
            • Neste função do created, com o axios estamos fazer uma requisição HTTP específica da url de cada pokemon.
            • Como resposta temos um série de dados, que especificamente neste código estamos guardando em uma array de objetos. Cada objeto tem as propriedades de cada pokemon. 

            • Front - imagem frontal
            • Back - imagem de costa
            • Type1 - tipo primario
            • Type2 - tipo secundario

            Essa verificação com o If precisou ser feita devido o tipo secundário que alguns pokemons não tem. Caso tentasse acessar o caminho na resposta que foi solicitada a propriedade não ia ser achada e por tanto a requisição ia falhar. Logo só pode ser definida e armazenada quando um tipo secundário existir, isso é o que está sendo testado.

            • MostrarTipoSecundario - é uma variável usada em um v-if no template para mostrar o tipo secundário do pokemon apenas quando existir.
          */
      })
    },
    data(){
      return{
        isFront:true,
        spriteAtual: '',
        propriedadesPokemon:{}
      }
    },
    props:{
      name:String,
      url:String,
    },
    methods:{
      mudarSprite: function(){

        if(this.isFront){
          this.spriteAtual = this.propriedadesPokemon.back;
          this.isFront = false;
        }else{
          this.spriteAtual = this.propriedadesPokemon.front;
          this.isFront = true;
        }
      }
    },
    computed:{
      nomePokemonFormatado: function(){
        return this.name.charAt(0).toUpperCase() + this.name.slice(1);
        
      //EXPLICAÇÃO:
      /*
        Esse método computado retorna o nome do pokemon com a primeira letra maisúscula
        */

      }
    }
  }
</script>

<style scoped>
.cartao{
  padding:20px;
  box-shadow: 5px 5px 10px 0px rgba(84, 84, 84, 0.321);
  width: 300px;
  border-radius:8px;
  background-color: #eeeeee;
}
.types{
  display: flex;
  justify-content: center;
  gap: 10px;
}
.types span{
  display:inline;
  padding: 0 12px;
  border-radius: 5px;
  color:white;
}
.grass{background-color: #63bc5a;}
.poison{background-color: #b567ce}
.bug{background-color: #91c12f}
.electric{background-color: #f4d23c}
.fire{background-color: #ff9d55}
.normal{background-color: #919aa2}
.rock{background-color: #c5b78c}
.dark{background-color: #5a5465}
.fairy{background-color:#ec8fe6}
.flying{background-color: #89aae3}
.ground{background-color: #d97845}
.steel{background-color: #5a8ea2}
.dragon{background-color: #0b6dc3}
.fighting{background-color: #ce416b} 
.ghost{background-color: #5269ad}
.ice{background-color: #73cec0}
.psychic{background-color: #fa7179 }
.water{background-color: #5090d6}

button{
  padding: 5px 12px;
  background-color: #eeeeee;
  border: 1px solid #aaa;
  border-radius: 5px;
}
button:hover{
  background-color:#cccccc;
  cursor: pointer;
}
</style>