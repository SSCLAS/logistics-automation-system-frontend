<template>
  <v-container>
    <v-responsive>
      <v-card title="출고 상태">
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
          <template v-slot:item.stock_order_processing="{ item }">
            <span>{{
              item.stock_order_processing ? "진행중" : "진행안됨"
            }}</span>
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
  {
    title: "제품 이름",
    key: "product_name",
    align: "center",
  },
  {
    title: "입고 명령 날짜",
    key: "stock_order_order_date",
    align: "center",
    value: (item) =>
      `${date.format(item.stock_order_order_date, "keyboardDateTime")}`,
  },
  {
    title: "입고 진행 상태",
    key: "stock_order_processing",
    align: "center",
  },
  {
    title: "입고 완료 날짜",
    key: "stock_order_complete_date",
    align: "center",
    value: (item) =>
      `${date.format(item.stock_order_complete_date, "keyboardDateTime")}`,
  },
];

// 각 product_id에 대해 제품 정보를 가져오는 함수
async function fetchProductDetails(productUrl) {
  try {
    const response = await axios.get(productUrl);
    if (response.status === 200) {
      return response.data;
    } else {
      console.log(`Failed to fetch product data from ${productUrl}`);
      return null;
    }
  } catch (error) {
    console.log(error);
    return null;
  }
}

// Products 조회 함수
async function getProducts() {
  try {
    const stockOrderResponse = await axios.get(
      "http://127.0.0.1:8000/Stock_order/"
    );
    if (stockOrderResponse.status === 200) {
      const stockOrders = stockOrderResponse.data;

      // 각 stock order에 대해 product 정보를 병합
      const productPromises = stockOrders.map(async (order) => {
        const productData = await fetchProductDetails(order.product_id);
        return {
          ...order,
          product_name: productData ? productData.product_name : "Unknown",
        };
      });

      // 모든 product 정보를 병합하여 최종 데이터 설정
      product_list.value = await Promise.all(productPromises);
      console.log("Product List:", product_list.value); // Debugging: Final product list
    } else {
      console.log("Failed to fetch stock order data");
    }
  } catch (error) {
    console.log(error);
  }
}

getProducts();
</script>
