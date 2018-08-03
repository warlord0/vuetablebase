<template>
  <div class="vuetable-pagination">
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

        <template v-if="notEnoughPages">
          <li
            v-for="n in totalPage"
            :key="n"
            :class="{'active': isCurrentPage(n)}"
            class="page-item"
          >
            <a
              class="page-link"
              @click.prevent="loadPage(n)"
              v-html="n"/>
          </li>
        </template>
        <template v-else>
          <li
            v-for="n in windowSize"
            :key="n"
            :class="{'active': isCurrentPage(windowStart+n-1)}"
            class="page-item"
          >
            <a
              class="page-link"
              @click.prevent="loadPage(windowStart+n-1)"
              v-html="windowStart+n-1"/>
          </li>
        </template>

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
