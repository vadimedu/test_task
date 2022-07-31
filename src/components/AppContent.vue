<template>
  <main class="container mt-md-5">

    <!-- Loading spinner -->
    <ui-spinner v-if="isLoading" />

    <template v-else>
      <ui-alert type="success" header="Data was cached!">
        <template #default>
          <p v-if="!show">You can clear cache and load data again by reloading page.</p>
          <p v-else>You clear cache successfully. Load again, please reload page</p>
        </template>
        <template #footer>
          <ui-button v-if="!show" type="primary" @click="clearCash">
            Clear cache
          </ui-button>
        </template>
      </ui-alert>
      <data-table
        :rows="data"
        :columns="columns">
      </data-table>
      <Money>
      </Money>
    </template>
  </main>
</template>

<script>
import { mapActions, mapState } from 'vuex';
import Money from './UI/Money.vue';

export default {
  name: 'AppContent',
  data: () => ({
    show: false,
    columns: [
      {
        label: 'ID',
        prop: 'id',
        width: '15%',
      },
      {
        label: 'Date',
        prop: 'ate',
        width: '20%',
      },
      {
        label: 'Name',
        prop: 'name',
        width: '30%',
      },
      {
        label: 'Money',
        prop: 'money',
        width: '35%',
      },
    ],
  }),
  computed: {
    ...mapState([
      'isLoading',
      'isCached',
      'data',
    ]),
  },
  created() {
    this.load();
  },
  methods: {
    ...mapActions([
      'load',
    ]),
    clearCash() {
      localStorage.clear();
      this.show = !this.show;
    },
  },
  components: { Money },
};
</script>
