<template>
    <main>
        <div>
            <h1>Liste des Clients</h1>
            <DataTable :value="data.listeClients" paginator :rows="5" tableStyle="min-width: 50rem">
                <Column field="code" header="Code" style="width: 25%"></Column>
                <Column field="societe" header="Société" style="width: 25%"></Column>
                <Column field="contact" header="Contact" style="width: 25%"></Column>
                <Column field="adresse.ville" header="Ville" style="width: 25%"></Column>
            </DataTable>
        </div>
    </main>
</template>

<script setup>
import { reactive, onMounted } from "vue";
import { doAjaxRequest } from "@/api";
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';

let data = reactive({
    listeClients: []
});

function showError(error) {
    console.log("Erreur : status %d", error.status)
    console.log(error.body);
    alert(error.message);
}

function chargeClients() {
    doAjaxRequest("/api/clients")
        .then((json) => {
            data.listeClients = json._embedded.clients;
        })
        .catch(showError);
}

//affichage composant = affichage de la liste
onMounted(chargeClients);

</script>