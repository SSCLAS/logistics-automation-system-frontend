<template>
  <v-container>
    <div>
      <div class="text-h5">{{ title }}</div>

      <v-data-table
        server
        :headers="headers"
        :items="items"
        class="table table-striped"
        ><thead :style="{ backgroundColor: '#343a40', color: 'white' }"></thead
      ></v-data-table>
    </div>
  </v-container>
</template>

<script>
export default {
  name: "TableComponent",
  props: {
    items: Array,
    headers: Array,
    title: String,
  },
};
</script>

<style scoped>
.v-container {
  max-width: 1200px;
  margin: auto;
  padding: 20px;
}

.text-h5 {
  margin-bottom: 20px;
}
</style>
