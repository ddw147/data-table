<template>
  <div>
    Data Table
    <template v-if="!isLoading">
      <table>
        <tr>
          <th v-for="header in headers" :key="header.name">
            {{ header.name }}
            <button @click="changeSort(header.value, 'ASC')">sort Asc</button>
            <button @click="changeSort(header.value, 'DESC')">sort Desc</button>
          </th>
        </tr>
        <tr v-for="item in currentItems" :key="item.id">
          <td v-for="header in headers" :key="header.name">
            <slot :name="header.name" v-bind:itemContent="item[header.value]">
              {{ item[header.value] }}
            </slot>
          </td>
        </tr>
      </table>
      <br />
      <div>
        <ul>
          <li v-for="page in numberOfPages" :key="page">
            <button @click="changePageTo(page)">{{ page }}</button>
          </li>
        </ul>
      </div>
    </template>

    <div v-else>Please Wait Dat is loading</div>
  </div>
</template>

<script>
export default {
  name: "DataTable",
  props: {
    headers: {
      type: Array,
      required: true,
    },
    items: {
      type: Array,
      required: true,
    },
    isLoading: {
      type: Boolean,
      required: false,
      default: false,
    },
    itemsPerPage: {
      type: Number,
      required: false,
      default: 10,
    },
  },
  data: function () {
    return {
      currentPage: 1,
      selectedSort: null,
      selectSortDirection: null,
    };
  },
  computed: {
    numberOfPages() {
      return Math.ceil(this.items.length / this.itemsPerPage);
    },
    currentItems() {
      let fromIndex = (this.currentPage - 1) * this.itemsPerPage;
      let toIndex = fromIndex + this.itemsPerPage;
      return this.sortedItems.slice(fromIndex, toIndex);
    },
    sortedItems() {
      if (this.selectedSort === null || this.selectSortDirection === null) {
        return this.items;
      }
      let sortedArray = [...this.items];

      return sortedArray.sort((firstItem, secondItem) => {
        let firstValue = firstItem[this.selectedSort];
        let secondValue = secondItem[this.selectedSort];

        let result =
          (typeof secondValue === "number") -
            (typeof firstValue === "number") ||
          (firstValue > secondValue ? 1 : -1);

        return this.selectSortDirection === "ASC" ? result : result * -1;
      });
    },
  },
  methods: {
    changePageTo(pageNo) {
      this.currentPage = pageNo;
    },
    changeSort(column, sortDirection) {
      this.selectedSort = column;
      this.selectSortDirection = sortDirection;
      this.currentPage = 1;
    },
  },
};
</script>
<style>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
  vertical-align: top;
}
table {
  width: 90%;
}
ul {
  display: inline;
  list-style-type: none;
  margin: 5px;
  padding: 2px;
}
li {
  display: inline;
  list-style-type: none;
  margin: 5px;
}
</style>
