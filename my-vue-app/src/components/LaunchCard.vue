<template>
  <div class="launch-card">
    <h3>{{ launch.name }}</h3>
    <p>Date : {{ new Date(launch.date_utc).toLocaleDateString() }}</p>
    <p>{{ launch.details || 'Aucun détail disponible.' }}</p>
    <p>Lieu : {{ launchSite || 'Chargement...' }}</p>
    <img 
      v-if="launch.links?.patch?.small" 
      :src="launch.links.patch.small" 
      alt="Mission Image"
    >
    <a 
      v-if="launch.links?.article" 
      :href="launch.links.article" 
      target="_blank"
    >
      Lire l'article
    </a>

    <!-- Bouton pour afficher la vidéo -->
    <button 
      v-if="launch.links.youtube_id" 
      @click="openVideo" 
      class="video-button">
      Voir la vidéo
    </button>

    <!-- Modale pour afficher la vidéo -->
    <div v-if="showModal" class="modal-overlay">
      <div class="modal-content">
        <iframe
          :src="youtubeEmbedUrl"
          frameborder="0"
          allowfullscreen
          class="video-frame"
        ></iframe>
        <button @click="closeVideo" class="close-button">Fermer</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['launch'],
  data() {
    return {
      showModal: false,
      launchSite: null, // Nom du lieu de lancement
    };
  },
  computed: {
    youtubeEmbedUrl() {
      return this.launch.links.youtube_id
        ? `https://www.youtube.com/embed/${this.launch.links.youtube_id}`
        : '';
    },
  },
  methods: {
    openVideo() {
      this.showModal = true;
    },
    closeVideo() {
      this.showModal = false;
    },
  },
  async mounted() {
    // Récupérer les informations du lieu de lancement via l'API
    if (this.launch.launchpad) {
      try {
        const response = await fetch(
          `https://api.spacexdata.com/v4/launchpads/${this.launch.launchpad}`
        );
        const data = await response.json();
        this.launchSite = data.name; // Mettre à jour le nom du lieu
      } catch (error) {
        console.error("Erreur lors de la récupération du lieu de lancement :", error);
        this.launchSite = "Lieu inconnu";
      }
    } else {
      this.launchSite = "Lieu non disponible";
    }
  },
};
</script>

<style scoped>
.launch-card {
  border: 1px solid #ddd;
  padding: 10px;
  margin: 50px;
}
.video-button {
  background-color: #007BFF;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  margin-top: 10px;
}
.video-button:hover {
  background-color: #0056b3;
}
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}
.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
  width: 90%;
  max-width: 600px;
}
.video-frame {
  width: 100%;
  height: 300px;
}
.close-button {
  background-color: red;
  color: white;
  border: none;
  padding: 5px 10px;
  margin-top: 10px;
  cursor: pointer;
}
.close-button:hover {
  background-color: darkred;
}
</style>

