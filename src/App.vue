<template>
  <div id="app">
    <DataTable
      :headers="headers"
      :isLoading="dataLoading"
      :items="tableData"
      :itemsPerPage="itemsPerPage"
    >
      <template v-slot:[`id`]="slotProps">
        {{ slotProps.itemContent }}
      </template>

      <template v-slot:[`title`]="slotProps">
        {{ slotProps.itemContent.toUpperCase() }}
      </template>
    </DataTable>
  </div>
</template>

<script>
import DataTable from "./components/DataTable.vue";
import axios from "axios";

export default {
  name: "App",
  components: { DataTable },
  data() {
    return {
      dataLoading: false,
      headers: [
        {
          name: "userId",
          value: "userId",
        },
        {
          name: "id",
          value: "id",
        },
        {
          name: "title",
          value: "title",
        },
        {
          name: "body",
          value: "body",
        },
      ],
      tableData: [],
      itemsPerPage: 10,
      sortColumn: null,
      sortDirection: null,
      selectedFont: "28px",
    };
  },
  async mounted() {
    this.dataLoading = true;
    try {
      let response = await axios.get(
        "https://jsonplaceholder.typicode.com/posts"
      );

      this.tableData = response.data;
    } catch (e) {
      console.log(e);
    } finally {
      this.dataLoading = false;
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 60px;
}
.selected {
  font-size: 28px;
}
</style>
