<template>
  <div class="vuetable-pagination-dropdown">
    <nav aria-label="Table Navigation">
      <ul class="pagination">

        <li
          :class="{'disabled': isOnFirstPage}"
          class="page-item">
          <a
            class="page-link"
            href="#"
            @click.prevent="loadPage(1)">
            <span><i class="fas fa-angle-double-left"/></span>
          </a>
        </li>

        <li
          :class="{'disabled': isOnFirstPage}"
          class="page-item">
          <a
            class="page-link"
            href="#"
            @click.prevent="loadPage('prev')">
            <span><i class="fas fa-angle-left"/></span>
          </a>
        </li>

        <div class="form-group mr-1 ml-1">
          <select
            id="paginationDropdown"
            class="form-control"
            @change="loadPage($event.target.selectedIndex+1)"
          >
            <template
              v-for="k in totalPage"
            >
              <option
                :key="k"
                :value="k"
                :selected="isCurrentPage(k)"
              >
                Page {{ k }}
              </option>
            </template>
          </select>
        </div>

        <li
          :class="{'disabled': isOnLastPage}"
          class="page-item"
        >
          <a
            class="page-link"
            href=""
            @click.prevent="loadPage('next')">
            <span><i class="fas fa-angle-right"/></span>
          </a>
        </li>

        <li
          :class="{'disabled': isOnLastPage}"
          class="page-item"
        >
          <a
            class="page-link"
            href=""
            @click.prevent="loadPage(totalPage)">
            <span><i class="fas fa-angle-double-right"/></span>
          </a>
        </li>

      </ul>
    </nav>
  </div>
</template>
<script>
import { VuetablePaginationMixin } from 'vuetable-2'
export default {
  mixins: [VuetablePaginationMixin]
}
</script>
