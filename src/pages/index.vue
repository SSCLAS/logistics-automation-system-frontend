<template>
  <v-container>
    <v-responsive>
      <v-card title="상품 배송 현황">
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
          <template v-slot:item.actions="{ item }">
            <v-btn
              color="primary"
              @click="update(item.url, item.product_available)"
            >
              {{ item.product_available ? "배송취소" : "배송" }}
            </v-btn>
          </template>
          <template v-slot:item.product_available="{ item }">
            <span>{{ item.product_available ? "배송됨" : "배송안됨" }}</span>
          </template>
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
  { title: "제품 배송 상태", key: "product_available", align: "center" },
  { title: "배송", key: "actions", align: "center", sortable: "false" },
];

// Products 조회 함수
async function getProducts() {
  try {
    const response = await axios.get("http://127.0.0.1:8000/Product/");
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

// product_available 업데이트
async function update(url, status) {
  try {
    const response = await axios.patch(url, {
      product_available: !status,
    });
    if (response.status == 200) {
      console.log("OK");
      getProducts();
    }
  } catch (error) {
    console.log(error);
  }
}

getProducts();
</script>
