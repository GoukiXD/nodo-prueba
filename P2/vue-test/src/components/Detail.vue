<template>
  <div>
    <h1 v-show="isLoading">
      Cargando personaje ...
    </h1>
    <table v-show="!isLoading">
      <tr>
        <th>Nombre</th>
        <th>Casa</th>
        <th>Slug</th>
        <th>Fue creado</th>
        <th>Sexo</th>
        <th>Ranking</th>

      </tr>
      <tr>

        <td>{{ character.name }}</td>
        <td>{{ character.house }}</td>
        <td>{{ character.slug }}</td>
        <td>{{ character.createdAt }}</td>

        
        <td v-if="character.male">Masculino</td>
        <td v-else>Femenino</td>  
        
        <td>{{ character.pageRank }}</td>
      </tr>
    </table>
    <table v-show="!isLoading">
      <tr>
        <th>Nombre Libro</th>
      </tr>
      <tr v-for="book in character.books">
        <td>{{ book }}</td>
      </tr>
    </table>
  </div>
</template>


<script>
  import { getACharacter } from '../services/got.service.js'

  export default {
    name: 'list-component',

    /**
     * @description the data block represents all the local variable of this component.
     */
    data () {
      return {
        character: {},
        isLoading: false
      }
    },

    /**
     * @description the create function is the first one to be execute when the component is being created (see vue js lifecycle).
     */
    created () {
        const id = this.$route.params.id;
        getACharacter(id)
        .then(res => {
          console.log('resp ', res);
          this.character = res.data
          this.isLoading = false
        })
    },

    /**
     * @description the methods block represents all the local methods of this component.
     */
    methods: {

      /**
       * @description get the detail of a character from the GoT API.
       * @param {string} id. the "_id" of the character that we are going to request.
       * @method goToDetail
       */
      goToDetail(id) {
        this.$router.push({
          name: 'detail', 
          params: {id: id}
          });
      }
    }
  }
</script>
<style>
  table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }
</style>