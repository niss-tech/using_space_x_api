<template>
  <div>
    <h2 class="text-2xl">Prochain Lancement</h2>
    <div v-if="nextLaunch" class="bg-gray-800 p-5 rounded-lg shadow-lg">
      <p class="text-lg font-bold">{{ nextLaunch.name }}</p>
      <p>Date : {{ formattedLaunchDate }}</p>
      <p>Décompte : {{ secondsLeft }} secondes</p>
      <img
        v-if="nextLaunch.links?.patch?.small"
        :src="nextLaunch.links.patch.small"
        alt="Mission patch"
        class="w-32 h-32 object-contain mt-3"
      />
      <p v-else class="text-gray-400">Aucune image disponible pour ce lancement.</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['nextLaunch'],
  data() {
    return {
      secondsLeft: 0,
      countdown: null,
    };
  },
  computed: {
    formattedLaunchDate() {
      if (!this.nextLaunch?.date_utc) return 'Date indisponible';
      const launchDate = new Date(this.nextLaunch.date_utc);
      return launchDate.toLocaleDateString('fr-FR', {
        day: '2-digit',
        month: '2-digit',
        year: 'numeric',
      });
    },
  },
  mounted() {
    if (this.nextLaunch) {
      const launchDate = new Date(this.nextLaunch.date_utc);

      this.countdown = setInterval(() => {
        const now = new Date();
        const diff = launchDate - now;
        this.secondsLeft = Math.floor(diff / 1000);

        if (this.secondsLeft <= 0) {
          clearInterval(this.countdown);
        }
      }, 1000);
    }
  },
  beforeUnmount() {
    if (this.countdown) {
      clearInterval(this.countdown);
    }
  },
};
</script>
