<template>
  <v-container>
    <v-responsive>
      <v-card title="제품 리스트">
        <template v-slot:text>
          <v-text-field
            v-model="search"
            label="Search"
            prepend-inner-icon="mdi-magnify"
            variant="outlined"
            hide-details
            single-line
          ></v-text-field>
        </template>

        <v-data-table
          :items="product_list"
          :headers="product_header"
          :search="search"
        >
        </v-data-table>
      </v-card>
    </v-responsive>
  </v-container>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useDate } from "vuetify";

const date = useDate();
const search = ref("");
const product_list = ref([]);
const product_header = [
  { title: "제품이름", key: "product_name", align: "center" },
  {
    title: "제품 제조 날짜",
    key: "product_date",
    align: "center",
    value: (item) => `${date.format(item.product_date, "keyboardDateTime")}`,
  },
  { title: "제품 가격", key: "product_price", align: "center" },
];

async function getProducts() {
  try {
    const response = await axios.get("http://192.168.0.100:8000/Product/");
    if (response.status == 200) {
      product_list.value = response.data;
      console.log("성공");
    } else {
      console.log("Failed to fetch product data");
    }
  } catch (error) {
    console.log(error);
  }
}
getProducts();
</script>
