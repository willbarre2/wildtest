<template>

  <header>
    <h1>
      <img src="https://www.wildcodeschool.com/assets/logo_main-e4f3f744c8e717f1b7df3858dce55a86c63d4766d5d9a7f454250145f097c2fe.png" alt="Wild Code School logo" />
      Les Argonautes
    </h1>
  </header>

  <main>
    
    <!-- New member form -->
    <h2>Ajouter un(e) Argonaute</h2>
    <form class="new-member-form">
      <label for="name">Nom de l&apos;Argonaute</label>
      <input id="name" name="name" type="text" placeholder="Charalampos" v-model="newName" :class="{ inputerror : !nameIsValid }" />
      <button type="submit" :disabled="!nameIsValid" @click.prevent="addName()">Envoyer</button>
    </form>
    
    <!-- Member list -->
    <h2>Membres de l'équipage</h2>
    <section id="member-list">
      <div class="member-item" v-for="argonaute in argonautes">{{argonaute.name}}</div>
    </section>
  </main>

  <footer>
    <p>Réalisé par Jason en Anthestérion de l'an 515 avant JC</p>
  </footer>
  
</template>


<script>
  export default {
    data(){
      return{
        argonautes: [],
        newName: "",
      };
    },
    mounted(){
      this.getNames();
    },
    computed: {
      // "!!" convertie en boolean pour tester si champ est rempli
      nameIsValid(){
        return !!this.newName
      },
    },
    methods:{
      
      getNames(){
        // requête pour affichage des noms
        const axios = require('axios').default;
        axios
        .get("http://localhost:5000/argonautes")
        .then((response) => {
          this.argonautes = response.data;
        })
        .catch((error) => console.log(error));
      },

      addName(){
        console.log(this.newName);
        // requête pour affichage des noms
        const axios = require('axios').default;
        axios
        .post("http://localhost:5000/argonautes/newname", {name: this.newName})
        .then((response) => {
          console.log("nom ajouté!");
          this.newName = "";
          this.getNames();
        })
        .catch((error) =>{
          console.log(error);
          alert("cette persone est déjà inscrite!");
          this.newName = "";
        });
      }
    }
}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');

body {
  margin: 0;
  font-family: 'Roboto', sans-serif;
}

main {
  max-width: 960px;
  margin: 0 auto;
}

header {
  background: #f4f4f4;
  text-align: center;
  padding: 2em;
}

header img {
  max-width: 96px;
}

header h1 {
  font-size: 2.5em;
}


h1, h2 {
  text-align: center;
}

label {
  display: block;
  margin-bottom: 0.5em;
}

input{
  margin: 0 1rem;
  padding: .4rem .6rem;
  border-radius: 5px;
  border: 1px solid black !important;

  &:focus{
    outline: none;
  }
}

button{
  font-weight: bold;
  padding: .3rem .6rem;
  border-radius: 5px;
  border: 2px solid #ff9f9f !important;
  color: white;
  background-color: #f76c6c;
  transition: all 0.2s ease-in-out;

  &:hover{
    cursor: pointer;
    background-color: yellowgreen ;
    border-color: rgb(183, 248, 52) !important;
    color: green;
    transform: scale(1.07);
  }

  &:disabled{
    cursor: not-allowed;
    color: rgb(189, 154, 154);
    background-color: #f76c6c7c;
    &:hover{
      border: 2px solid #ff9f9f !important;
      transform: scale(1);
    }
  }
}

.inputerror{
  border: 1px solid rgb(255, 32, 32) !important;
}

.new-member-form {
  margin: 2em 0 4em 0;
  text-align: center;
}

#member-list{
  width: 50%;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;

  .member-item {
    width: 33.3%;
    margin: 1rem 0;
  }
}


footer {
  margin-top: 2em;
  text-align: center;
  color: #fff;
  background: #f76c6c;
  padding: 0.25em 0;
}

</style>
