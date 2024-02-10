<template>
<div>
<table>
  <caption>Liste des clients</caption>
  <tr>
    <th>Code</th>
    <th>Société</th>
    <th>Contact</th>
    <th>Ville</th>
  </tr>
  <!-- Si le tableau des catégories est vide -->
  <tr v-if="data.listeCategories.length === 0">
    <td colspan="4">Veuillez patienter, chargement des catégories...</td>
  </tr>
  <!-- Si le tableau des catégories n'est pas vide -->
  <tr v-for="categorie in data.listeCategories" :key="categorie.code">
    <td>{{ categorie.code }}</td>
    <td>{{ categorie.societe }}</td>
    <td>{{ categorie.contact }}</td>
    <td>{{ categorie.adresse.ville }}</td>
  </tr>
  <tr>
    <td><button type="number" @click="chargeCategories(data.listeLinks.first.href)">Debut</button></td>
    <td><button v-if="typeof data.listeLinks.prev !== 'undefined'" type="number" @click="chargeCategories(data.listeLinks.prev.href)">Précédente</button></td>
    <td><button v-if="typeof data.listeLinks.next !== 'undefined'" type="number" @click="chargeCategories(data.listeLinks.next.href)">Suivante</button></td>
    <td><button type="number" @click="chargeCategories(data.listeLinks.last.href)">Fin</button></td>

  </tr>
</table>
</div>

</template>

<script setup>
import {onMounted, reactive} from "vue";
import {doAjaxRequest} from "@/api";

//reinitialisation formulaire
const categorieVide = {
  libelle: "",
  description: ""
};

const page = 0;

let data = reactive({
  // données formulaire
  formulaireCategorie: {...categorieVide},
  // liste des catégories sous forme de table
  listeCategories: [],
  listeLinks: []
});

function showError(error) {
  console.log("Erreur : status %d", error.status)
  console.log(error.body);
  alert(error.message);
}
function chargeCategories(urlPage) {
  // Appel à l'API pour liste des catégories
  urlPage = urlPage.substring(urlPage.indexOf("/api"));
  doAjaxRequest(urlPage)
      .then((json) => {
        data.listeCategories = json._embedded.clients;
        data.listeLinks = json._links;
        console.log(data.listeLinks);
      })
      .catch(showError);
}

onMounted(() => {
  chargeCategories("/api/clients?&page=0&size=5");
})

</script>

<style scoped>
td,
th {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #232623;
  color: rgb(255, 255, 255);
}
</style>