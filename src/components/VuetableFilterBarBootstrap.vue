<template>
  <div class="vuetable-filter-bar">
    <div class="form-inline">
      <div class="form-group">
        <label class="sr-only">Search for:&nbsp;</label>
        <div class="input-group">
          <div class="input-group-prepend">
            <span class="input-group-btn">
              <button
                class="btn btn-primary"
                type="button"
                @click="doFilter"
              >
                <i class="fas fa-search"/>
              </button>
            </span>
          </div>
          <input
            v-model="filterText"
            class="form-control"
            placeholder="Search string"
            type="text"
            @keyup.enter="doFilter"
          >
          <div class="input-group-append">
            <span class="input-group-btn">
              <button
                class="btn btn-default"
                type="button"
                @click="resetFilter"
              >
                <i class="fas fa-times"/>
              </button>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      filterText: ''
    }
  },
  methods: {
    doFilter () {
      this.$events.fire('filter-set', this.filterText)
    },
    resetFilter () {
      this.filterText = ''
      this.$events.fire('filter-reset')
    }
  }
}
</script>
<style>
.form-group {
  width: 100%;
}
</style>
