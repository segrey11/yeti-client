<template>
  <div>
    <h3 class="text-left pl-5">
      Rates
    </h3>
    <DataTable
      :fields="fields"
      :items="rates"
      :rows="rows"
      :get-data="getRates"
    >
      <template v-slot:filter>
        <!-- <RatesFilter v-on:applyFilter="getRates" /> -->
      </template>
    </DataTable>
  </div>
</template>

<script>
import { flow, get } from 'lodash';
// import RatesFilter from './RatesFilter';
import DataTable from '../DataTable/DataTable';
import utils from '../../utils';
import { RATES } from '../../constants';
import { TABLE_HEADERS } from './constants';

export default {
  name: 'Rates',
  components: {
    // RatesFilter,
    DataTable,
  },
  data() {
    return {
      fields: TABLE_HEADERS,
    };
  },
  computed: {
    rates() {
      return flow(utils.formatRates)(this.$store.getters.rates.items);
    },
    rows() {
      return get(this.$store.getters, ['rates', 'meta', 'total-count'], 0);
    },
  },
  created() {
    this.getRates();
  },
  methods: {
    getRates(pageNumber) {
      this.$store.dispatch(RATES.ACTIONS.GET_RATES, pageNumber);
    },
  },
};
</script>

<style>
</style>
