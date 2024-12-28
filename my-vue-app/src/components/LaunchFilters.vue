<template>
  <div class="relative w-full max-w-xs mx-auto mb-5">
    <label 
      for="launchFilter" 
      class="block text-sm font-medium text-gray-300 mb-2"
    >
    </label>
    <select
      id="launchFilter"
      v-model="statusFilter"
      @change="updateFilter"
      class="block w-full bg-gray-800 text-gray-300 border border-gray-600 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
    >
      <option value="all">Tous les lancements</option>
      <option value="success">Réussis</option>
      <option value="failed">Échoués</option>
    </select>
  </div>
</template>

<script>
export default {
  props: ['currentFilter'],
  data() {
    return {
      statusFilter: this.currentFilter || 'all',
    };
  },
  methods: {
    updateFilter() {
      this.$emit('filter-changed', this.statusFilter);
    },
  },
};
</script>