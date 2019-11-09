<template>
  <div id="dataTable">
    <slot name="filter" />
    <div class="dataTable">
      <slot
        name="quickFilter"
      />

      <h6
        v-if="!loading"
        class="datatable-total"
      >
        Items in table: {{ rows }}
      </h6>
      <b-table
        :busy="loading"
        :small="small"
        :items="items"
        :per-page="perPage"
        :striped="striped"
        :fixed="fixed"
        :fields="fields"
        sticky-header="calc(100vh - 17rem)"
        hover
        show-empty
      >
        <template
          v-slot:table-busy
        >
          <div
            class="text-center text-dark my-1"
          >
            <b-spinner
              class="align-middle"
            >
            </b-spinner>

            <strong>Loading...</strong>
          </div>
        </template>

        <template
            v-slot:empty="scope"
            style="font-size: 11px"
        >
          <h4>{{ scope.emptyFilteredText }}</h4>
        </template>

        <template
          :slot="badgedItem"
          slot-scope="row"
        >
          <b-badge
            :variant="row.item[badgedItem] ? 'success' : 'danger'"
            pill
          >
            {{ row.item[badgedItem] }}
          </b-badge>
        </template>
      </b-table>
      <b-pagination
        v-model="currentPage"
        :disabled="loading"
        :total-rows="rows"
        :per-page="perPage"
        align="center"
        size="sm"
        aria-controls="dataTable"
        @change="getData"
      />
    </div>
  </div>
</template>


<script>
export default {
  name: 'Cdrs',
  props: {
    fields: {
      type: Array,
      default() {
        return [];
      },
    },
    items: {
      type: Array,
      default() {
        return [];
      },
    },
    rows: {
      type: Number,
      default: 0,
    },
    badgedItem: {
      type: String,
      default: '',
    },
    getData: {
      type: Function,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      small: false,
      striped: true,
      fixed: false,
      perPage: 50,
      currentPage: 1,
    };
  },
  computed: {
    loading() {
      return this.$store.getters.isRequestPending;
    },
  },
};
</script>


<style>
.dataTable {
  font-size: 12px;
  white-space: nowrap;
  position: relative;
}

.dataTable .b-table-empty-row {
  height: 1000px;
}

.dataTable .datatable-total {
  position: absolute;
  top: 0.5rem;
  right: 15px;
}

.dataTable .pagination {
  padding-left: 15px;
}
</style>
