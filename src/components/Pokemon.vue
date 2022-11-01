<template>
  <div id="pokemon">

    <div class="card">
      <div class="card-image pokemon-img">
        <figure>
          <img :src="currentImg" alt="Placeholder image"/>
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content pokemon-card">
            <p class="title is-4">{{ index }} - {{ name | upper }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>
        <div class="content">
            <button @click="mudarSprite" class="button is-medium is-fullwidth">Mudar Sprite</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created() {
    axios
      .get(this.url)
      .then((res) => {
        this.pokemon.type = res.data.types[0].type.name;
        this.pokemon.front = res.data.sprites.front_default;
        this.pokemon.back = res.data.sprites.back_default;
        this.currentImg = this.pokemon.front
        console.log(this.pokemon);
      })
      .catch((err) => console.error(err));
  },

  data() {
    return {
        isFront: true,
        currentImg: '',
        pokemon: {
          type: '',
          front: '',
          back: ''
      },
    };
  },

  props: {
    index: Number,
    name: String,
    url: String,
  },
  filters: {
    upper: function (value) {
      const newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  methods: {
      mudarSprite() {
          if (this.isFront) {
            this.isFront = false;
            this.currentImg = this.pokemon.back       
          } else {
              this.isFront = true;
              this.currentImg = this.pokemon.front
          }
      }
  }
};
</script>

<style scoped>

#pokemon {
    margin-top: 2%;
}

.pokemon-img {
    margin-left: 290px;
}

.pokemon-card {
    margin-left: 15em;
}


</style>