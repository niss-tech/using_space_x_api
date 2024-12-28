<template>
  <div>
    <h2 class="text-2xl">Liste des lancements</h2>
    <div v-if="filteredLaunches.length">
      <LaunchCard
        v-for="launch in limitedLaunches"
        :key="launch.id"
        :launch="launch"
      />
    </div>
    <div v-else>
      <p>Aucun lancement trouvé.</p>
    </div>
  </div>
</template>

<script>
import LaunchCard from './LaunchCard.vue';

export default {
  props: ['launches', 'statusFilter'],
  components: { LaunchCard },
  computed: {
    // Lancements filtrés en fonction du statut sélectionné
    filteredLaunches() {
      if (this.statusFilter === 'all') {
        return this.launches;
      }
      return this.launches.filter(
        launch => launch.success === (this.statusFilter === 'success')
      );
    },
    // Limiter aux 10 derniers lancements
    limitedLaunches() {
      return this.filteredLaunches
        .sort((a, b) => new Date(b.date_utc) - new Date(a.date_utc)) // Trier par date décroissante
        .slice(0, 10); // Prendre les 10 premiers
    },
  },
};
</script>


