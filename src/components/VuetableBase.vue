/**
 * vuetable-base
 * Import this component and pass the following props
 *   api-url = Api URL returns json in Laravel pagination format
 *   fields-def = Array of field definitions to show in the table
 *   sort-order = Array sort.field and sort.direction
 *   row-clicked = Function to run when a row is clicked - passed row.item
 *
 *  eg.
 *    <vuetable api-url="/data" :row-clicked="rowClicked"></vuetable>
 *
 * @type {component}
 */
<template>
  <div id="vuetableBase">
    <div>
      <div
        class="row">
        <vuetable-filter-bar
          class="col-12 col-sm-6"
        />
        <!-- Api call should return standard Laravel paginate() json -->
        <vuetable-pagination
          ref="pagination"
          class="col-12 col-sm-6"
          @vuetable-pagination:change-page="onChangePage"
        />
      </div>
      <!-- Generate the table based on the default api call -->
      <vuetable
        ref="vuetable"
        :fields="fieldsDef"
        :css="css"
        :sort-order="sortOrder"
        :append-params="moreParams"
        :http-fetch="getData"
        :api-url="apiUrl"
        :per-page="perPage"
        class="table-striped table-hover"
        pagination-path=""
        @vuetable:row-clicked="rowClicked"
        @vuetable:pagination-data="onPaginationData"
        @vuetable:loading="onLoading"
        @vuetable:loaded="onLoaded"
      />

      <div class="row">
        <!-- Api call should return standard Laravel paginate() json -->
        <vuetable-pagination-dropdown
          ref="paginationDropdown"
          class="col-12 offset-sm-6 col-sm-6"
          @vuetable-pagination:change-page="onChangePage"
        />
      </div>

    </div>

  </div>
</template>

<script>
import Vuetable from 'vuetable-2'
// Bootstrap 4 style pagination
import VuetablePagination from './VuetablePaginationBootstrap'
import VuetablePaginationDropdown from './VuetablePaginationDropdownBootstrap'
import VuetableCss from './VuetableCss'

import VuetableFilterBar from './VuetableFilterBarBootstrap'

export default {
  components: {
    Vuetable, // The vuetable
    VuetablePagination, // nav/button based pagination
    VuetablePaginationDropdown, // select based pagination
    VuetableFilterBar // Search
  },
  props: {
    apiUrl: {
      type: String,
      default: ''
    },
    fieldsDef: {
      type: Array,
      default: function () {
        return []
      }
    },
    sortOrder: {
      type: Array,
      default: function () {
        return [{
          field: 'id',
          direction: 'asc'
        }]
      }
    },
    rowClicked: {
      type: Function,
      default: function () {
        return
      }
    },
    perPage: {
      type: Number,
      default: 10
    }
  },
  data () {
    return {
      loading: true, // component status
      css: VuetableCss, // Reusable Css
      moreParams: {} // Sends the filter with the other params
    }
  },
  computed: {
  },
  created () {

  },
  mounted () {
    // Listen for the filter events from VuetableFilterBar
    this.$events.$on('filter-set', eventData => this.onFilterSet(eventData))
    this.$events.$on('filter-reset', e => this.onFilterReset()) // eslint-disable-line no-unused-vars
  },
  methods: {
    /* Search Filter Handling */
    onFilterSet (filterText) {
      // console.log('filter-set', filterText)
      this.moreParams = {
        filter: filterText
      }
      // console.log(this.$refs.vuetable)
      process.nextTick( () => this.$refs.vuetable.refresh() )
    },
    onFilterReset () {
      // console.log('filter-reset')
      this.moreParams = {}
      process.nextTick( () => this.$refs.vuetable.refresh() )
    },
    // replaces the vuetales own call to api-url to handle jwt auth
    getData (apiUrl, httpOptions) {
      return this.$http.get(apiUrl, httpOptions)
    },
    // Trigger the pagination changes
    onPaginationData (paginationData) {
      this.$refs.pagination.setPaginationData(paginationData)
      this.$refs.paginationDropdown.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    },
    onLoading () {
      // console.log('loading')
      this.loading = true
    },
    onLoaded () {
      // console.log('loaded')
      this.loading = false
    },
  },
}
</script>
<style>
.vuetable-pagination nav {
  float: right;
}
.vuetable-pagination-dropdown nav {
  float: right;
}
</style>
