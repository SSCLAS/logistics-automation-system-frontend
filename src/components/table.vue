<template>
  <v-container>
    <div>
      <div>
        <div class="text-h5">{{ title }}</div>
        <table class="custom-table">
          <thead>
            <tr>
              <th v-for="header in headers" :key="header.value">
                {{ header.text }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in items" :key="item.url" class="table-row">
              <td v-for="header in headers" :key="header.value">
                {{ item[header.value] }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </v-container>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
    headers: {
      type: Array,
      required: true,
    },
    title: {
      type: String,
      default: "",
    },
  },
};
</script>

<style scoped>
.table-title {
  margin-bottom: 20px;
  text-align: left;
  font-size: 24px;
  font-weight: bold;
}

.custom-table {
  width: 100%;
  margin-top: 20px;
  border-collapse: collapse;
}

.custom-table th,
.custom-table td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.custom-table th {
  background-color: #343a40;
  color: white;
}
</style>
