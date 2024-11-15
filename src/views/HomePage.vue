<template>
  <ion-page>
    <!-- Centrado del título principal -->
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title class="center-title">AyussitoMusic</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">AyussitoMusic</ion-title>
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
              <!-- Barra de búsqueda -->
              <ion-searchbar placeholder="Buscar artista..." @ionInput="buscarArtista"></ion-searchbar>
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
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonTabs, IonTab, IonTabBar, IonTabButton, IonIcon, IonSearchbar } from '@ionic/vue';
import { playCircle, radio, library, search, peopleOutline, musicalNotesOutline } from 'ionicons/icons';

const icons = {
  playCircle,
  radio,
  library,
  search,
  peopleOutline,
  musicalNotesOutline,
};

onMounted(async () => {
  await cargarDatos();
});

interface Artista {
  artista: string;
  desc: string;
  foto: string;
}

const datos = ref<Artista[]>([]);
const datosFiltrados = ref<Artista[]>([]);

const cargarDatos = async () => {
  const ruta = "src/artistas.json";
  fetch(ruta)
    .then(response => response.json())
    .then(data => {
      datos.value = data.artistas;
      datosFiltrados.value = data.artistas;
    })
    .catch(error => {
      console.log("Hubo errores cargando", error);
    });
};

const buscarArtista = (event: any) => {
  const query = event.target.value.toLowerCase();
  datosFiltrados.value = datos.value.filter((artista) => artista.artista.toLowerCase().includes(query));
};
</script>

<style scoped>
.center-title {
  text-align: center;
}

.scrollable-list {
  max-height: 300px;
  overflow-y: auto;
}

.example-content {
  padding: 20px;
  font-size: 16px;
}

ion-content {
  --background: #f4f5f8;
}
</style>
