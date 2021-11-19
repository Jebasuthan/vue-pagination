<template>
  <ul class="pagination">
    <li class="pagination-item">
      <a href="#" @click.prevent="onClickFirstPage" :class="isInFirstPage? 'disabled':''" :disabled="isInFirstPage">First</a>
    </li>
    <li class="pagination-item">
      <a href="#" @click.prevent="onClickPreviousPage" :class="isInFirstPage? 'disabled':''" :disabled="isInFirstPage">«</a>
    </li>
    <li :key="page.id" v-for="page in pages" class="pagination-item">
      <a href="#" @click.prevent="onClickPage(page.name)" :disabled="page.isDisabled"
        :class="{ active: isPageActive(page.name) }">{{ page.name }}</a>
    </li>
    <li class="pagination-item">
      <a href="#" @click.prevent="onClickNextPage" :class="isInLastPage? 'disabled':''" :disabled="isInLastPage">»</a>
    </li>
    <li class="pagination-item">
      <a href="#" @click.prevent="onClickLastPage" :class="isInLastPage? 'disabled':''" :disabled="isInLastPage">Last</a>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'pagination',
  props: {
    maxVisibleButtons: {
      type: Number,
      required: false,
      default: 3
    },
    totalPages: {
      type: Number,
      required: true
    },
    perPage: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    }
  },
  computed: {
    isInFirstPage () {
      return this.currentPage === 1
    },
    isInLastPage () {
      if (this.totalPages === 0) {
        return true
      }
      return this.currentPage === this.totalPages
    },
    startPage () {
      // When on the first page
      if (this.currentPage === 1) {
        return 1
      }
      // When on the last page
      if (this.totalPages < this.maxVisibleButtons) {
        return 1
      }
      if (this.currentPage === this.totalPages) {
        return this.totalPages - this.maxVisibleButtons + 1
      }
      // When in between
      return this.currentPage - 1
    },
    endPage () {
      if (this.totalPages === 0) {
        return 1
      }
      if (this.totalPages < this.maxVisibleButtons) {
        return this.totalPages
      }
      return Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages)
    },
    pages () {
      const range = []
      for (let i = this.startPage; i <= this.endPage; i++) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage
        })
      }
      return range
    }
  },
  methods: {
    onClickFirstPage () {
      if (this.isInFirstPage) {
        return false
      }
      this.$emit('pagechanged', 1)
    },
    onClickPreviousPage () {
      if (this.isInFirstPage) {
        return false
      }
      this.$emit('pagechanged', this.currentPage - 1)
    },
    onClickPage (page) {
      this.$emit('pagechanged', page)
    },
    onClickNextPage () {
      if (this.isInLastPage) {
        return false
      }
      this.$emit('pagechanged', this.currentPage + 1)
    },
    onClickLastPage () {
      console.log('onClickLastPage')
      if (this.isInLastPage) {
        return false
      }
      this.$emit('pagechanged', this.totalPages)
    },
    isPageActive (page) {
      return this.currentPage === page
    }
  }
}
</script>
<style lang="scss" scoped>
  .pagination {
    list-style-type: none;
    float: right;
    margin: 10px 0;
    .pagination-item {
      display: inline-block;
      color: #ddd;
      a {
        text-decoration: none;
        margin: 5px;
        color: #2c3e50;
      }
      a.disabled {
        color: #ccc;
        cursor: no-drop;
      }
      .active {
        background-color: tomato;
        color: #ffffff !important;
        font-weight: bold;
        padding: 3px 8px;
      }
    }
  }
  button[disabled], html input[disabled] {
    cursor: default;
    color: lightgray;
  }
</style>
