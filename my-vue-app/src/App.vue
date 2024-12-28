<template>
  <div class="min-h-screen bg-black p-4">
    <header class="mb-8">
      <h1 class="text-left text-5xl font-bold text-blue-500">Space X Tracker</h1>
    </header>
    
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <div class="lg:col-span-1">
        <NextLaunch :nextLaunch="nextLaunch" />
        <LaunchFilters 
          :currentFilter="statusFilter" 
          @filter-changed="setFilter" 
          class="mt-4"
        />
      </div>
      
      <div class="lg:col-span-2">
        <LaunchList 
          :launches="launches" 
          :statusFilter="statusFilter" 
        />
      </div>
    </div>
  </div>
</template>

<script>
import NextLaunch from './components/NextLaunch.vue';
import LaunchFilters from './components/LaunchFilters.vue';
import LaunchList from './components/LaunchList.vue';

export default {
  components: { NextLaunch, LaunchFilters, LaunchList },
  data() {
    return {
      nextLaunch: null,
      launches: [],
      statusFilter: 'all',
    };
  },
  async mounted() {
    // Récupérer le prochain lancement
    const nextLaunchResponse = await fetch('https://api.spacexdata.com/v4/launches/next');
    this.nextLaunch = await nextLaunchResponse.json();

    // Récupérer tous les lancements
    const launchesResponse = await fetch('https://api.spacexdata.com/v4/launches');
    this.launches = await launchesResponse.json();
  },
  methods: {
    setFilter(newFilter) {
      this.statusFilter = newFilter;
    },
  },
};
</script>


