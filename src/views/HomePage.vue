<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>AyussitoMusic</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-tabs>
        <ion-tab tab="home">
          <ion-card> 
            <ion-card-header>
              <ion-card-title>Lista</ion-card-title>
              <ion-card-subtitle>Pide a tu artista favorito!</ion-card-subtitle>
            </ion-card-header>

            <ion-card-content>
              <!-- Contenedor con scroll y altura fija -->
              <div class="scrollable-list">
                <ion-list>
                  <ion-item v-for="artista in datos" :key="artista.artista">
                    <ion-thumbnail slot="start">
                      <img alt="Foto" :src="artista.foto" />
                    </ion-thumbnail>
                    <ion-label>{{ artista.artista }}</ion-label>
                    <ion-button>Ver canciones</ion-button>
                    <ion-button>Enviar</ion-button>
                  </ion-item>
                </ion-list>
              </div>
            </ion-card-content>
          </ion-card>
        </ion-tab>

        <ion-tab tab="radio">
          <div id="radio-page">
            <ion-header>
              <ion-toolbar>
                <ion-title>Radio</ion-title>
              </ion-toolbar>
            </ion-header>
            <ion-content>
              <div class="example-content">Radio content</div>
            </ion-content>
          </div>
        </ion-tab>
        <ion-tab tab="library">
          <div id="library-page">
            <ion-header>
              <ion-toolbar>
                <ion-title>Library</ion-title>
              </ion-toolbar>
            </ion-header>
            <ion-content>
              <div class="example-content">Library content</div>
            </ion-content>
          </div>
        </ion-tab>
        <ion-tab tab="search">
          <div id="search-page">
            <ion-header>
              <ion-toolbar>
                <ion-title>Search</ion-title>
              </ion-toolbar>
            </ion-header>
            <ion-content>
              <div class="example-content">Search content</div>
            </ion-content>
          </div>
        </ion-tab>

        <ion-tab-bar slot="bottom">
          <ion-tab-button tab="home">
            <ion-icon :icon="icons.peopleOutline" />
            Artistas
          </ion-tab-button>
          <ion-tab-button tab="radio">
            <ion-icon :icon="icons.musicalNotesOutline" />
            Canciones del artista
          </ion-tab-button>
          <ion-tab-button tab="library">
            <ion-icon :icon="icons.library" />
            Biblioteca
          </ion-tab-button>
          <ion-tab-button tab="search">
            <ion-icon :icon="icons.search" />
          </ion-tab-button>
        </ion-tab-bar>
      </ion-tabs>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonTabs, IonTab, IonTabBar, IonTabButton, IonIcon } from '@ionic/vue';
import { playCircle, radio, library, search, peopleOutline, musicalNotesOutline } from 'ionicons/icons';

const icons = {
  playCircle,
  radio,
  library,
  search,
  peopleOutline,
  musicalNotesOutline,
};

onMounted(async () => { // cuando se arranca la app
  await cargarDatos(); // llama a cargar datos
});

// Definimos las propiedades de cada artista
interface Artista {
  artista: string;
  desc: string;
  foto: string;
}

// Donde vamos a volcar el JSON para que sea parte de la app
const datos = ref<Artista[]>([]); // Define el tipo explícitamente

// Funcion para cargar datos
const cargarDatos = async () => {
  const ruta = "src/artistas.json";
  // Cargar JSON
  fetch(ruta)
    .then(response => response.json())
    .then(data => {
      datos.value = data.artistas;
      console.log(datos.value);
      console.log(datos.value[0].artista); // Ejemplo de acceso a los datos
    })
    .catch(error => {
      console.log("Hubo errores cargando", error);
    });
};
</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

.scrollable-list {
  max-height: 500px; 
  overflow-y: auto;  
}

.example-content {
  padding: 20px;
  font-size: 16px;
}

ion-content {
  --background: #0a41e583; 
}
</style>
